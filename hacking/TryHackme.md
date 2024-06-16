
- Try Hack me: maquinas virtuales remotas
- HackmeVM: maquinas virtuales locales
- vulnhub: maquinas virtuales locales
- Hack the box: maquinas virtuales remotas, sin roll tab
- vulnix: maquinas virtuales locales

# HackTheBox

plataforma de seguridad informática que ofrece un entorno de aprendizaje y práctica para mejorar habilidades en seguridad informática. 


## Conexion VPN

Conectarse a la red interna de tryHAckme o HackTheBox a traves de un tunel virtual VPN, este se genera usando un archivo .ovpn


## Prueba de penetración

Al realizar por primera vez una prueba de penetración lo primero que se debe considerar es documentar el estado actual de la maquina objetivo para aprender de ella tanto como sea posible.

## Fases de penetración

## Escaneo

Lo primero al estudiar un dispositivo ajeno es el escaneo de sus puertos con el objetivo de ver el rol del dispositivo (servicios que tiene) y si tiene vulnerabilidades con una herramienta "nmap". nmap (network mapper): envia paquetes a los puertos con la esperanza de recibir respuestas de ellos lo que determina si estan abiertos o no. Alguno puertos se utilizan por defecto por ciertos servicios y otros no son tan evidentes.

```bash
# version de servicio
$ nmap -sV {target_IP}
# una vez escaneados los puertos, investigar si estos ofrecen informacion del servicio que operan

```




