# Capítulo 13: Creación de modelos para MongoDB con Mongoose

## Instalar las dependencias de producción

1. Abrir la terminal de la Raspberry Pi desde Visual Studio Code.
2. Ejecutar `cd proyecto-raspberrypi-iot`.
3. Ejecutar `cd api`.
4. Ejecutar `npm install mongoose`.

📝[mongoose](https://www.npmjs.com/package/mongoose).

## Crear un archivo JS para cada modelo

1. Ejecutar `mkdir models`.
2. Ejecutar `cd models`.
3. Ejecutar `touch device.model.js`.
4. Ejecutar `touch telemetry.model.js`.
5. Ejecutar `touch action.model.js`.

## Crear el modelo en device.model.js

1. Ver el sistema de archivos de la Raspberry Pi desde Visual Studio Code.
2. Abrir el archivo `device.model.js`.
3. Modificar el contenido del `device.model.js`:

```
import mongoose from "mongoose"

const deviceSchema = new mongoose.Schema(
    {
        name: String,
        type: String
    }
)

export const deviceModel = mongoose.model("Device", deviceSchema)
```

## Crear el modelo en telemetry.model.js

1. Abrir el archivo `telemetry.model.js`.
2. Modificar el contenido del `telemetry.model.js`:

```
import mongoose from "mongoose"

const telemetrySchema = new mongoose.Schema(
    {
        day: Date,
        device: String,
        nsamples: Number,
        first: Number,
        last: Number,
        telemetry: JSON
    }
)

export const telemetryModel = mongoose.model("Telemetry", telemetrySchema)
```

## Crear el modelo en action.model.js

1. Abrir el archivo `action.model.js`.
2. Modificar el contenido del `action.model.js`:

```
import mongoose from "mongoose"

const actionSchema = new mongoose.Schema(
    {
        device: String,
        command: String,
        parameter: String,
        ts: Date
    }
)

export const actionModel = mongoose.model("Action", actionSchema)
```

## Modificar el entry point

1. Abrir el archivo `index.js`.
2. Modificar el contenido del `index.js`:

```
import express from "express"
import mongoose from "mongoose"
import { deviceRouter } from "./routes/device.routes.js"
import { telemetryRouter } from "./routes/telemetry.routes.js"
import { actionRouter } from "./routes/action.routes.js"

const app = express()
const port = 3000

const connectToMongoDB = async () => {
    try {
        await mongoose.connect("mongodb://user:pass@localhost:27017/", {
            dbName: "iot"
        })
        console.log("La API se conectó a la base de datos de MongoDB")
    } catch (error) {
        console.error(`Falló la conexión a la base de datos de MongoDB: ${error}`)
    }
}

app.use("/device", deviceRouter)
app.use("/telemetry", telemetryRouter)
app.use("/action", actionRouter)

connectToMongoDB()

app.listen(port, () => {
    console.log(`La API esta funcionando en el puerto ${port}`)
})
```

## Probar la conexión entre la API y MongoDB

1. Ejecutar `cd ..`.
2. Ejecutar `docker compose up -d`.
3. Ejecutar Node en modo desarrollo o producción.

![Conexión de la API con MongoDB](1.png)
