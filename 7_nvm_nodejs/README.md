# Capítulo 7: nvm y nodejs

## Instalar nvm en la Raspberry Pi

1. Ejecutar `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash`.
2. Cerrar la terminal.
3. Abrir una nueva terminal.
4. Ejecutar `nvm ls-remote` para ver las versiones de nodejs disponibles.

![nvm ls-remote](1.png)

📝[nvm](https://github.com/nvm-sh/nvm).

## Instalar la última versión LTS de nodejs con nvm en la Raspberry Pi

1. Ejecutar `nvm install 22.11.0`.
2. Ejecutar `nvm use 22.11.0`.
3. Ejecutar `node -v`.

![nodejs](2.png)
