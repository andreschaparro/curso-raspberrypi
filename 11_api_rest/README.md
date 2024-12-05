# Capítulo 11: API REST para MongoDB con nodejs en la Raspberry Pi

## Crear el proyecto de nodejs

1. Abrir la terminal de la Raspberry Pi desde Visual Studio Code.
2. Ejecutar `cd proyecto-raspberrypi-iot`.
3. Ejecutar `mkdir api`.
4. Ejecutar `cd api`.
5. Ejecutar `npm init`.
6. No modificar el `package name` y presionar `ENTER`.
7. No modificar la `version` y presionar `ENTER`.
8. Ingresar `Gestión de ingreso de telemetrías (TIM) y llamadas a procedimientos remotos (RPC)` como `description` y presionar `ENTER`.
9. No modificar el `entry point` y presionar `ENTER`.
10. No modificar el `test command` y presionar `ENTER`.
11. No modificar el `git repository` y presionar `ENTER`.
12. No modificar las `keywords` y presionar `ENTER`.
13. Ingresar nuestro nombre como `author`y presionar `ENTER`.
14. No modificar la `license` y presionar `ENTER`.
15. Presionar `ENTER`.

Como resultado, se crea el archivo `package.json`.

## Instalar las dependencias de desarrollo

1. Ejecutar `npm install --save-dev nodemon`.

📝[nodemon](https://www.npmjs.com/package/nodemon).

## Instalar las dependencias de producción

1. Ejecutar `npm install express`.
2. Ejecutar `npm install body-parser`.
3. Ejecutar `npm install dotenv --save`.
4. Ejecutar `npm install mqtt --save`.

📝[express](https://www.npmjs.com/package/express).

📝[body-parser](https://www.npmjs.com/package/body-parser).

📝[dotenv](https://www.npmjs.com/package/dotenv).

📝[mqtt](https://www.npmjs.com/package/mqtt).

## Crear el entry point del proyecto

1. Ejecutar `touch index.js`.

## Modificar los scripts de nodejs para el proyecto

1. Ver el sistema de archivos de la Raspberry Pi desde Visual Studio Code.
2. Abrir el archivo `package.json`.
3. Eliminar el script llamado `test`.
4. Agregar un script llamado `dev` que ejecute `nodemon index.js`.
5. Agregar un script llamado `start` que ejecute `node index.js`.

## Habilitar la importación/exportación del tipo ES6

1. Abrir el archivo `package.json`.
2. Agregar un atributo llamado `type` con el valor `module`.

![package.json](1.png)

## Trabajar en modo desarrollo

1. Ejecutar `npm run dev`.

## Ejecutar el proyecto en modo producción

1. Ejecutar `npm start`.
