## Introducción:

En la era digital actual, nuestras vidas dependen en gran medida de las redes informáticas. Desde el trabajo y las comunicaciones hasta el entretenimiento y las compras, accedemos a una gran cantidad de información sensible a través de internet. La seguridad de la red se ha convertido en un aspecto fundamental para proteger estos recursos valiosos de las amenazas internas y externas.

## ¿Qué es la seguridad de la red?

La seguridad de la red es un conjunto de medidas y prácticas diseñadas para proteger los recursos de una red informática, como los datos, los equipos y los sistemas. Su objetivo principal es garantizar la confidencialidad, integridad y disponibilidad de la información que se transmite y almacena en la red.

### Estandar 802.1x 

El estandar IEEE realiza el control de acceso de una red, mediante la autenticación que habilita o impide el acceso en redes cableadas o inalámbricas, **1x hace referencia al potocolo de autenticación extensible EAP** entre dispositivos finales e intermedios ademas de los servidores de autenticación como el **radius**.

## Asociación y transmisión

Asociación.- Un usuario elige la SSID (red wi-fi), el dispositivo contacta con el AP que ofrece esa SSID, se negocian varias caracteristicas pero sobre todo el AP solicita algun tipo de autenticación para asociar el recurso de internete o no.

Transmisión.- Una vez asociado con un AP, comienza la fase de transmisión. Para proteger los paquetes durante la transmisión se debe cifrar por medio de los protocolo como WPA o WPA2

Para el proceso de autenticación se debe distinguir entre 2 ambitos: empresarial y personal.

En el ambito personal es suficiente con una unica clave PSK [pre shared key], en el ambito empresarial se usa un servidor especifico de autenticación (radius) que almacen a una lista de usuario-clave. 

### RADIUS (Remote Autentication Dial-In User Server) 1812 UDP

Es un protocolo que nos permite gestionar la autenticación, autorización y registro AAA (Authentication, Authorization y Accounting), este tiene una BD de usuario y contraseña.

Funcionamiento: los servidores radius tienen un registro de los IP que que pertenecen a lo AP que pueden enviar una solicitud al servidor, cuando un cliente solicita asociarse con un AP, envía la solicitud al AP y este le solicita usuario y contraseña, pero no las comprueba en si mismo sino que envia esta solicitud al sevidor radius para que la procese y dependiendo de la respuesta el AP admita o rechaze la solicitud de asociación.










