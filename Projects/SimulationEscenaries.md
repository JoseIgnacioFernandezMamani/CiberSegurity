# Simulación de un entorno de trabajo

Requisitos:

1. instalación OPNsense 24.1 firewall de red: [enlace de descarga OPNsense](https://opnsense.org/download/) 

Este debe cumplir con las tareas de:

- **Generar las redes LAN, WAN adémas de una DMZ.**
- **Servidor DHCP** para asignar las IPs dinámicas a los clientes de la LAN.
- **La dirección LAN a 192.168.x.y /24.**
- **La DMZ a 172.16.x.y/24.**
- Usar usuarios para **portal cautivo**, cuando el usuario se autentique el portal cautivo debera **redireccionar a uatf.edu.bo**
- **limitar el ancho de banda a los usuarios**, para tener niveles de usuario con relación al uso de internet. Usar una pagina para medir el ancho de banda.

### Permisos Firewall

- ninguna PC debe poder acceder a ninguna pagina web para adultos.
- Ninguna de las PCs debe poder descargar archivos con extension .exe, .iso, .mkv.
- Ninguna PC debe poder acceder a Youtube ni a Facebook.

2. **Implementar IDS snort o suricata** en una maquina virtual con alguna distribución de linux, además debe **integrarla a A.C.I.D. base o GRAYLOG** para tener un registro de trafico y un entorno grafico.

utilizar 10.24.x.y/24 

2 reglas para probar su funcionamiento

- La primera que genere una alerta, cuando su PC ingrese al sitio web [softonic](https://www.softonic.com)

- La segunda que genere una alerta para detectar cuando alguien este realizando ping a la dirección ip 8.8.8.8 de google.

3. **Implementar un sistema de monitoreo de redes con los agentes Zabbix o Nagios**, a partir de la cual, debé monitorear 2 clientes; un cliente con S.O. windows y un segundo con un S.O. linux

De cada cliente realice el monitoreo de 1 servicio o proceso:

- Para windows, cuando desactive el antivirus en la pc que se monitorea, se debera reportar a Zabbix para su verificación.

- Para Linux, que reporte cuando se desintala un programa








