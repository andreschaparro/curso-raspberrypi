# Capítulo 6: Mosquitto MQTT broker

## Instalar las herramientas de Mosquitto en la PC

1. Instalar [Mosquitto Broker](https://mosquitto.org/download/).
2. Ir a las variables de entorno de Windows.
3. Ingresar a la variable del sistema llamada `Path`.
4. Agregar `C:\Program Files\mosquitto`.

![Variable Path](1.png)

5. Ir a los servicios de Windows.
6. Ingresar a las propiedades del servicio llamado `Mosquitto Broker`.
7. Clic en `Detener`.
8. Seleccionar `Deshabilitado` en `Tipo de inicio`.
9. Clic en `Aceptar`.

![Servicio Mosquitto Broker](2.png)

📝[Mosquitto Documentation](https://mosquitto.org/documentation/).

## Crear la estructura de carpetas que necesita Mosquitto en la Raspberry Pi

1. Abrir la terminal de la Raspberry Pi por SSH.
2. Abrir el sistema de archivos de la Raspberry Pi por SSH.
3. Ejecutar `mkdir proyecto-raspberrypi-iot`.
4. Ejecutar `cd proyecto-raspberrypi-iot`.
5. Ejecutar `mkdir docker`.
6. Ejecutar `mkdir transporte`.
7. Ejecutar `cd transporte`.
8. Ejecutar `mkdir mosquitto`.
9. Ejecutar `cd mosquitto`.
10. Ejecutar `mkdir config`.
11. Ejecutar `mkdir data`.
12. Ejecutar `mkdir log`.

📦[eclipse-mosquitto](https://hub.docker.com/_/eclipse-mosquitto).

## Crear el archivo mosquitto.conf de Moquitto en la Raspberry Pi

1. Ejecutar `cd config`.
2. Ejecutar `wget https://raw.githubusercontent.com/eclipse-mosquitto/mosquitto/master/mosquitto.conf`.
3. Descomentar la línea 234.
4. Modificar su contenido a `listener 1883`.
5. Descomentar la línea 428.
6. Modificar su contenido a `persistence true`.
7. Descomentar la línea 438.
8. Modificar su contenido a `persistence_location /mosquitto/data/`.
9. Descomentar la línea 471.
10. Modificar su contenido a `log_dest file /mosquitto/log/mosquitto.log`.
11. Descomentar la línea 479.
12. Descomentar la línea 480.
13. Descomentar la línea 481.
14. Descomentar la línea 482.
15. Descomentar la línea 532.
16. Modificar su contenido a `allow_anonymous true`.

📝[mosquitto.conf man page](https://mosquitto.org/man/mosquitto-conf-5.html).

## Crear el archivo Dockerfile de Moquitto en la Raspberry Pi

1. Ejecutar `cd ..`.
2. Ejecutar `touch Dockerfile`.
3. Modificar el contenido del `Dockerfile`:

![Dockerfile](3.png)

📝[Dockerfile overview](https://docs.docker.com/build/concepts/dockerfile/).
📝[Dockerfile reference](https://docs.docker.com/reference/dockerfile/).

## Crear el archivo compose.yaml de Docker en la Raspberry Pi

1. Ejecutar `cd ..`.
2. Ejecutar `cd ..`.
3. Ejecutar `cd docker`.
4. Ejecutar `touch compose.yaml`.
5. Modificar el contenido del `compose.yaml`:

![compose.yaml](4.png)

📝[Docker Compose](https://docs.docker.com/compose/).

## Crear y arrancar el contenedor de Mosquitto en la Raspberry Pi

1. Ejecutar `docker compose up -d` para crear y arrancar los contenedores de Docker en segundo plano.

## Subscribirse y publicar un mensaje al topic llamado mensajes desde la PC

1. Abrir una terminal.
2. Ejecutar `mosquitto_sub -h XXX.XXX.XXX.XXX -t "mensajes" -p 1883 -v`. Donde `XXX.XXX.XXX.XXX` es la dirección IP de la Raspberry Pi.
3. Abrir una terminal.
4. Ejecutar `mosquitto_pub -h XXX.XXX.XXX.XXX -t "mensajes" -m "Hola Mundo" -p 1883`. Donde `XXX.XXX.XXX.XXX` es la dirección IP de la Raspberry Pi.

![mosquitto_sub y mosquitto_pub](5.png)

5. Cerrar las terminales.

📝[mosquitto_sub man page](https://mosquitto.org/man/mosquitto_sub-1.html).

📝[mosquitto_pub man page](https://mosquitto.org/man/mosquitto_pub-1.html).

## Detener el contenedor de Mosquitto en la Raspberry Pi

1. Ejecutar `docker compose stop` para detener los contenedores de Docker.
