# Capítulo 5: Docker

## Instalar Docker en la Raspberry Pi

1. Abrir la terminal de la Raspberry Pi por SSH.
2. Ejecutar `curl -fsSL https://get.docker.com -o get-docker.sh`.
3. Ejecutar `sudo sh get-docker.sh`.
4. Ejecutar `sudo groupadd docker`.
5. Ejecutar `sudo usermod -aG docker $USER`.
6. Ejecutar `sudo reboot`.
7. Esperar a que se reinicie la Raspberry Pi.

📝[Install using the convenience script](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script).

📝[Linux post-installation steps for Docker Engine](https://docs.docker.com/engine/install/linux-postinstall/).

## Crear y arrancar un contenedor de Docker de prueba en la Raspberry Pi

1. Abrir la terminal de la Raspberry Pi por SSH.
2. Ejecutar `docker run hello-world` para verificar que Docker se instaló correctamente.

![docker run hello-world](1.png)

3. Ejecutar `docker compose version` para verificar que Docker Compose se instaló correctamente.

![docker compose version](2.png)

📝[Docker Compose](https://docs.docker.com/compose/).

📝[CLI Cheat Sheet](https://docs.docker.com/get-started/docker_cheatsheet.pdf).

## Instalar una extensión para Visual Studio Code en la PC

1. Instalar la extensión [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).