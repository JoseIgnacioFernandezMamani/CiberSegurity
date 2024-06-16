# cron

Es un servicio de S.O. unix/linux que permite a los usuarios programar tareas periodicas para que se ejecuten automaticamente, las tareas se conocen como "crons".
## instalacion 

[how install cron](https://www.digitalocean.com/community/tutorials/how-to-use-cron-to-automate-tasks-ubuntu-1804)

A partir de herramientas cron es posible programar mediante cripts horas, fechas concretas para realizar copias de seguridad.

## Understanding How Cron Works

Los trabajos cron se registran y administran en un archivo especial conocido como crontab. Cada perfil de usuario en el sistema puede tener su propio crontab donde puede programar trabajos, que se almacena en /var/spool/cron/crontabs/.

Para programar un trabajo, abra su crontab para editarlo y agregue una tarea escrita en forma de expresión cron. La sintaxis de las expresiones cron se puede dividir en dos elementos: la programación y el comando a ejecutar.

El comando puede ser prácticamente cualquier comando que normalmente ejecutaría en la línea de comando. El componente de programación de la sintaxis se divide en 5 campos diferentes, que se escriben en el siguiente orden:

``` bash
[programación en 5 campos] [comando]
minute hour day_of_month month day_of_week command_to_run
```

ejemplo:
``` bash
30 17 * * 2 curl http://www.google.com
```
Esta expresión ejecuta el comando curl http://www.google.com todos los martes a las 5:30 p. m.

- *: En expresiones cron, un asterisco es una variable comodín que representa "todos". Así, una tarea programada con * * * * * ... se ejecutará cada minuto de cada hora de cada día de cada mes.
- ,: Las comas dividen los valores de programación para formar una lista. Si desea ejecutar una tarea al principio y a la mitad de cada hora, en lugar de escribir dos tareas separadas (por ejemplo, 0 * * * * ... y 30 * * * * ...), puede lograr lo siguiente: misma funcionalidad con uno (0,30 * * * * ...).
- -: Un guión representa un rango de valores en el campo de programación. En lugar de tener 30 tareas programadas separadas para un comando que desea ejecutar durante los primeros 30 minutos de cada hora (como en 0 * * * *..., 1 * * * *..., 2 * * * * .. ., y así sucesivamente), en su lugar, podría programarlo como 0-29 * * * * ....

- /: Puede utilizar una barra diagonal con un asterisco para expresar un valor de paso. Por ejemplo, en lugar de escribir ocho tareas cron separadas para ejecutar un comando cada tres horas (como en, 0 0 * * * ..., 0 3 * * * ..., 0 6 * * * ..., y así sucesivamente), puedes programarlo para que se ejecute así: 0 */3 * * * ....

Nota: No se pueden expresar valores de paso de forma arbitraria; sólo puedes utilizar números enteros que se dividan uniformemente dentro del rango permitido por el campo en cuestión. Por ejemplo, en el campo "horas" solo puede seguir una barra diagonal con 1, 2, 3, 4, 6, 8 o 12.

A continuación se muestran algunos ejemplos más de cómo utilizar el componente de programación de cron:

* * * * * - Ejecute el comando cada minuto.
12 * * * * - Ejecute el comando 12 minutos después de cada hora.
0,15,30,45 * * * * - Ejecute el comando cada 15 minutos.
*/15 * * * * - Ejecuta el comando cada 15 minutos.
0 4 * * * - Ejecute el comando todos los días a las 4:00 a. m.
0 4 * * 2-4: ejecuta el comando todos los martes, miércoles y jueves a las 4:00 a. m.
20,40 */8 * 7-12 * - Ejecute el comando los minutos 20 y 40 de cada 8 horas todos los días de los últimos 6 meses del año.

Si algo de esto le resulta confuso o si desea ayuda para escribir cronogramas para sus propias tareas cron, Cronitor proporciona un práctico editor de expresiones de cronograma cron llamado "Crontab Guru" que puede utilizar para verificar si sus cronogramas cron son válidos.

## Administrar crontabs

Una vez que haya establecido un cronograma y sepa el trabajo que desea ejecutar, deberá colocarlo en algún lugar donde su daemon pueda leerlo.

**Un crontab es un archivo especial que contiene el cronograma de trabajos que seran ejecutados por cron**. Sin embargo, estos no están pensados ​​para editarse directamente. En su lugar, se recomienda utilizar el comando crontab. Esto le permite editar el crontab de su perfil de usuario sin cambiar sus privilegios con sudo. El comando crontab también le permitirá saber si tiene errores de sintaxis en el crontab, mientras que editarlo directamente no lo hará.

Puedes editar tu crontab con el siguiente comando:
```bash
$ crontab -e
```
Si es la primera vez ejecutando se le pedira elegir el editor de texto de preferencia

Cuando ejecute crontab -e en el futuro, aparecerá su crontab en este editor de texto automáticamente. 

Si desea ver el contenido de su crontab, pero no editarlo, puede usar el siguiente comando:
```bash
$ crontab -l
```
Puedes borrar tu crontab con el siguiente comando:
```bash
$ crontab -r
```

### Práctica 
pasos:
1. Entrar modo super usuario
```bash
$ sudo -i
```
2. Entrar en el directorio /tmp y crear un archivo:
```bash
$ cd /tmp
$ mkdir original && cd original
$ echo texto en documento > texto.txt
$ ls -l
```
3. Realizar una copia o backup completo a otro directorio
```bash
$ rsync -av /tmp/original /tmp/copia
# se creara el directorio copia si no fue creado anteriormente, y se copiaran solo los ficheros nuevos o que cambiaron y no todos como fuese el caso de usar $ cp
$ cd ..
# busca el directorio y visualiza su contenido
$ find original
# [resultado]
$ find copia
# [resultado]
```
4. ahora veremos los backups incrementales
crear un directorio /tmp/backup1 y añadirle un texto textoBackup.txt

Ahora procedemos a realizar el backup incremental
```bash
$ rsync -avvb --backup-dir=/tmp/backup1 --delete /tmp/original /tmp/copia  
# sintaxis dir=[almacenamiento del backup] --[] [el archivo en seguimiento] [otro archivo en seguimiento]
```

--delete: eliminar los archivos en el directorio de destino (/tmp/copia) si no existen en el directorio de origen (/tmp/original). 
está sincronizando el contenido del directorio /tmp/original con el directorio /tmp/copia, asegurándose de que los archivos en el destino estén respaldados en /tmp/backup1 antes de ser modificados o eliminados, y eliminando cualquier archivo en el destino que no exista en el origen

5. crear un ejecutable mybackup.sh con permisos de ejecución

```bash
$ chmod u+rwx script.sh
```
el script:
```bash
#!/bin/cdsh
FECHA=$(date +%y%m%d%H%M)
rsync -avvb --backup-dir=/tmp/backup1 --delete /tmp/original /tmp/copia >> /tmp/log_$FECHA
```

6. Ejecutar crontab
```bash
$ crontab -e
```
7. Se abrira el archivo en un editor de texto, si es la primera vez se le solicitara elegir, crear la programació y comandos.
```bash
* * * * * /tmp/mybackup.sh
```

8. iniciar el crontab.
```bash
$ sudo /etc/init.d/cron start
```

si se desea apagar el servicio
```bash
$ sudo /etc/init.d/cron stop
```

Listo, se creara un backup de los archivos y directorios indicados segun la regla que hayamos colocado



Consideraciones finales, para enviar los logs a otro equipo por una red interna.

Pasos para configurar la transferencia de archivos
1. instalar ssh con los siguiente comandos para la máquina virtual que tendrá el rol de servidor:
```bash
$ sudo apt-get update -> actualizar la lista de paquetes ubuntu
$ sudo apt-get upgrade -> actualizar los paquetes a su versión mas moderna
$ sudo apt install openssh-server -> instalar el servidor openssh
$ sudo systemctl enable ssh -> habilitar la función ssh
$ sudo ufw allow ssh -> habilitar el puerto 22 en el firewall ufw
# si está apagado encender con:
$ sudo systemctl start ssh 
# verificar que este encendido el servidor ssh
$ sudo systemctl status ssh
```

2. instalar ssh con los siguiente comandos para la máquina anfitrión que tendrá el rol de cliente:
```bash
sudo apt-get update
sudo apt-get upgrade 
sudo apt install openssh-client
º -> habilitar la función ssh
sudo ufw allow ssh -> habilitar el puerto 22 en el firewall ufw
# si está apagado encender con:
sudo systemctl start ssh 
# verificar que este encendido el servidor ssh
sudo systemctl status ssh
```

3. Para acceder a las maquinas de forma remota

```bash
$ ssh host@ip
```

4. Enviar archivos

```bash
$ scp [directorio-origen] host@ip:[directorio-destino]
```
5. Qué pasa si me he olvidado mi host ?
ejecutamos el comando:

```bash
$ whoami
```

6. para evitar la autenticación constante cada que se trata de enviar por scp, se optó por crear una clave ssh de mi maquina virtual y copiarlo en mi maquina anfitrion

se dio el siguiente proceso:
```bash
# generamos clave en la maquina virtual
$ ssh-keygen
# este comando te solicitará un lugar donde guardar la clave (si no has especificado uno) y una frase de contraseña opcional para proteger  clave privada.

# compartimos la clave publica con la maquina anfitrión
$ ssh-copy-id host@ip
# este ultimo almacenara la clave publica en el directorio de mi maquina anfitrion, (genericamente es: 	~/.ssh/authorized_keys), 
y se puede acceder simplemente con ssh usuario@ip a mi maquina anfitrion, pero no colocar passphrase.
```
Ya podemos conectarno sin autorización desde la maquina virtual a la maquina anfitrion

Consideraciones finales:
scp -r para envio recursivo
