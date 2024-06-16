# burp suite


## Conceptos generales

- www (world wide web): Infraestructura que consta conjunto de nodos interconectados. Esta se compone de:

- lenguaje que estructura la informacion (HTML).
- lenguajes de programacion para la logica.
- protocolo que se encarga de la carga y descarga de datos como http y https.
- localizadores de recursos de la web url (uniform resource location).
- interfaces para interpretar el codigo recibido de internet (navegadores).


## Concepto

- dominio: identificador unico de un recurso de la web asociando direcciones IP con un conjunto de caracteres(nombre).
- DNS: protocolo de resolución de direcciones IP a dominios UDP 53.

Los dominios tienen jerarquia:
- TLD (top level domain): dominios primarios de internet, van al final de la url, ej : .com .bo .edu, son administrados por la ICANN.
- SLD (second level domain): son los dominios secundarios que identifican a un sitio web en la red.
- subdominios: son la parte que indica el direccionamiento de los recursos de un sitio web.

Una **url** no puede apuntar a 2 recursos a la vez y se compone de:

[protocolo][:// {segmento delimitador}][dominio][/recurso][puerto-(opcional)][?parametros de consulta][#(ancla)analiza secciones]

https://url.com/recurso:443?parametro
 
## Diferencia entre un sitio web y pagina web

- Pagina web: documento singular identificada por una url
- Sitio web: conjunto de paginas web que estan relacionadas y agrupadas por un mismo dominio.


## Hoisting, VPS y servidor dedicado

- Hoisting: opcion mas comun utilizada para servir sitios web, multiples sitios web que comparte un mismo servidor que es arendado por los clientes compartiendo recursos de hardware del servidor.
- Servidor dedicado: los recursos se disponen a un solo cliente.
- VPS (virtual private server): Es como el hosting solo que a determinados clientes se les asigna recursos dedicados.

## Frontend, Backend y DB

- Frontend: Es el diseño visual de una pagina web.
- Backend: Es la logica de procesamiento del sitio web.
- DB (data base): Es donde se almacenan los datos de l negocio.

## Servidores 

- Software que acepta solicitudes , procesa y responde. El mas usado es apache, ngix, IIS, apache tomcat aplicaciones para java y JETTY no consume tantos recursos.
- CMS (content manager system): permite montar un sitio web a traves de una herramienta pre configurada. Principales tipos de CMS: 

- WordPress (PHP, MySql o MariaDB). 
- Joomla (PHP, MySql).
- Drupal.

## Metodos de invio de información 

- GET: solicitud  de datos, no altera el estado del recurso, envia la solicitud a traves de la url en los parametros de la misma por lo que son visiblesl. Operaciones de lectura.
- POST: Envia datos para ser procesados, envia datos en el cuerpo de la solicitud por lo que no son visibles. Crear recursos como a traves de un formulario.
- PUT: Enviar datos para actualizar todo de un recurso existente, remplaza datos existentes con nuevos enviados.    
- DELETE: eliminar un recurso del servidor.
- PATCH: Aplica actualizaciones parciales de un recurso existente.
- HEAD: solicitud d e metadatos.
- TRACE: muestra la ruta que toma una solicitud.
- CONNECT: convierte la colicitud de conexion en un metodo TCP/IP
- OPTION: determina las caracteristicas de un recurso o servidor.

## Cabeceras HTTP

Son componentes que transmiten informacion adicional entre el cliente y el servidor.

- HOST: identifica el dominio y el puerto que el cliente solicita de un servidor.
- USER-AGENT: identifica el navegador y SO del cliente que realiza la solicitud.
- ACCEPT: formato de respuesta.
- ACCEPT LANGUAJE: especifica el idioma del cliente
- ACCEPT ENCODING: especifica los formatos de codificacion que el cliente acepta para la comprension de datos.
- Content type: Especifica el tipo de datos enviados en el cuerpo de la solicitud.
- Content leng: especifica el tamaño de la solicitud.
- cookie: contiene las cookies.
- connection: especifica si la conexión debe mantenerse abierta o cerrada despues de la solicitud.
- referer: indica la url de origen desde el cual se accedio al recurso solicitado.
- authorization: especifica las credenciales para autenticar al cliente con el servidor.
- cache control: especifica directivas para la cache tanto para solicitudes como las respuestas, conrtolando como se almacenan y recuperan los datos.
- x-fordwarded-for: indica la IP del cliente que inica la solicitud.
- set-cookie: crear cookies.
- x-xss-protection: controla la funcion de proteccion de scripts.

## Codigos de estado HTTP

Indicacion del resultado de la solicitud hecha. Categorias: 

- 1xx: respuestas informativas. 100 
- 2xx: respuestas satisfactorias. 200 ok, 201 create (se ha creado el recurso), 204 (se ha completado con exito pero no nos entrega datos)
- 3xx: redirecciones. 301 se movio el recurso pernanentemente, 302 el recurso esta ubicado en otra dirección.
- 4xx: errores del cliente. 400 solicitud no se pudo entender,  401 no se tiene permisos par aacceder a los recursos solicitados,  403 forbiden elservidor si le llego pero se niega a responder,  404 el recurso solicitado no fue encontrado.
- 5xx:  errores del lado del servidor. 500 error interno del ervidor, 502 el servidor recibio una respuesta invalida y no pudo reponder a la solicitud, 503 el servidor no se encuentra disponible por un tiempo.

## Burp suite: 

Suite de herramientas orientadas al pentesting web desarrolada por la empresa port swiger.

Herramientas:

- proxy: interceptar, inspeccionar y modificar el trafico http o https y ponerse en el medio entre el navegador y el servidor.
- intruder:  automatizar acciones manuales de la herramienta.
- repeter: envia las peticiones repetidamente de acuerdo a configuraciones.

### Partes de bup suite

- Dashboard: panel de control tiene: tasks (tareas que se realizan), new scan, new live task (tarea nueva),  
- target: 
- proxy:
- intruder:
- repeater:
- collaborator: 
- sequencer: 
- decoder: 
- compare: 
- logger: 
- organizer:
- extension:
- learn: 

## Proxy de burp suite

### Proxy

Es un software que opera como un punto intermedio entre el navegador y el servidor, todas la solicitudes del navegador pasan por el proxy para filtrar datos que son enviados o que son recibidos, este se encarga de principalemtne ocultar la IP para no mostrar esta informacion directamente al servidor.

































