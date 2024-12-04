# Capítulo 5: Asignarle una dirección IP fija a la Raspberry Pi

## Obtener la dirección MAC de la interfaz wifi de la Raspberry Pi

1. Abrir la terminal de la Raspberry Pi desde Visual Studio Code.
2. Ejecutar `ifconfig`.
3. Buscar la interfaz llamada `wlan0`.
4. Buscar el atributo llamado `ether` que contiene la dirección MAC.

📝Una dirección MAC tiene el formato XX:XX:XX:XX:XX:XX, done X es un número hexadecimal.

## Reservar la dirección IP de la Raspberry Pi

1. Enviar la dirección IP al administrador de la red wifi.
2. Enviar la dirección MAC al administrador de la red wifi.
3. Solicitarle al administrador de la red wifi, que la dirección IP que le enviamos quede reservada para la Raspberry Pi.
4. 🤞.
