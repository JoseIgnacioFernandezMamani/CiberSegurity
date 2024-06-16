| Herramienta | Tablas              | Cadenas predefinidas                            |
| ----------- | ------------------- | ----------------------------------------------- |
| arptables   | filter              | PREROUTING, INPUT, FORWARD, OUTPUT, POSTROUTING |
| ebtables    | filter              | PREROUTING, INPUT, FORWARD, OUTPUT, POSTROUTING |
| ip6tables   | filter, mangle, raw | INPUT, FORWARD, OUTPUT                          |
| iptables    | filter, nat, mangle | INPUT, OUTPUT, FORWARD                          |

FORWARD: paquetes que fueron enrutados.

sintaxis basica de iptables

targets (acciones)

DROP: descartar
ACCEPT: paquete continua su camino

```bash
# Estructura:
iptables [-t tabla] -[opciones] [chain/regla]
# OPCIONES:
#    -A: añade una regla al final de la lista
#    -I: inserta una regla al comienzo de la lista o en el punto especificado
#    -R: reemplaza una regla (especificada por su número de regla) por otra
#    -D: borra una regla determinada
#    -P: politica por defecto
# PARAMETROS DE LAS REGLAS:
#    -p [!] protocolo: el protocolo del paquete a comprobar. Puede ser ‘tcp’, ‘udp’, ‘icmp’ o ‘all’
#    -[sd] [!] dirección[/máscara]: dirección ip origen (s) o destino (d) del paquete
#    -[io] [!] iface: nombre de la interfaz de entrada (i) o de salida (o) del paquete
#    -j target(acción): especifica el target de dicha regla. Puede ser una acción predefinida (ACCEPT, DROP), una extensión o el nombre de una chain
#    -t indica el nombre de la tabla


# comando necesarios
#
# Cuenta cuantas veces una regla ha coincidido con un paquete.
iptables [-t tabla] -Z [chain]
# Borrado de todas la reglas de la cadena
iptables [-t tabla] -F [chain]
# F == flush (borrar)
```

## Establecimiento de un política por defecto:

```bash
iptables [-t table] -P chain target
# P == policy
```

## Almacenar las reglas

```bash
# para guardar de forma sencilla es 
itpables-save
# guardar las reglas en un archivo con reglas
iptables-save > archivo_conreglas.rules
# Configurar las reglas de forma permanente en el S.O.
iptables-restore < archivo_conreglas.rules
# lista las reglas
iptables -t [tabla] -L [cadena]
# elimina las reglas
iptables -F [tabla]

```
