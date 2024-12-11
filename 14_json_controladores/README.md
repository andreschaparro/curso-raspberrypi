# Capítulo 14: Controladores que reciben y devuelven JSON

## Instalar las dependencias de producción

1. Abrir la terminal de la Raspberry Pi desde Visual Studio Code.
2. Ejecutar `cd proyecto-raspberrypi-iot`.
3. Ejecutar `cd api`.
4. Ejecutar `npm install body-parser`.

📝[body-parser](https://www.npmjs.com/package/body-parser).


## Crear un archivo JS para cada controlador

1. Ejecutar `mkdir controllers`.
2. Ejecutar `cd controllers`.
3. Ejecutar `touch device.controller.js`.
4. Ejecutar `touch telemetry.controller.js`.
5. Ejecutar `touch action.controller.js`.

## Crear el controlador en device.controller.js

1. Abrir el archivo `device.controller.js`.
2. Modificar el contenido del `device.controller.js`:

```
import { Device } from "../models/device.model.js"

export const getAllDevices = async (req, res) => {
    try {
        const devices = await Device.find()

        if (devices.length === 0) {
            return res.status(204).json([])
        }

        res.json(devices)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al solicitar los dispositivos" })
    }
}

export const createDevice = async (req, res) => {
    const { name, type } = req.body

    if (!name || !type) {
        return res.status(400).json({ message: "Los campos name y type son obligatorios" })
    }

    try {
        const existingDevice = await Device.findOne({ name })

        if (existingDevice) {
            return res.status(409).json({ message: "El dispositivo ya existe" })
        }

        const device = new Device({
            name,
            type
        })

        const newDevice = await device.save()

        res.status(201).json(newDevice)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al crear el dispositivo" })
    }
}
```

## Crear el controlador en telemetry.controller.js

1. Abrir el archivo `telemetry.controller.js`.
2. Modificar el contenido del `telemetry.controller.js`:

```
import { Telemetry } from "../models/telemetry.model.js"

export const getAllTelemetries = async (req, res) => {
    const { device } = req.params

    try {
        const telemetries = await Telemetry.find({ device })

        if (telemetries.length === 0) {
            return res.status(204).json([])
        }

        res.json(telemetries)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al solicitar todas las telemetrías" })
    }
}

export const getOneDayTelemetries = async (req, res) => {
    const { device, day } = req.params

    try {
        const telemetries = await Telemetry.find({
            device,
            day
        })

        if (telemetries.length === 0) {
            return res.status(204).json([])
        }

        res.json(telemetries)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al solicitar las telemetrías de un día" })
    }
}

export const getFromToTelemetries = async (req, res) => {
    const { device, from, to } = req.params

    try {
        const telemetries = await Telemetry.find({
            device,
            day: { $gte: from, $lte: to }
        })

        if (telemetries.length === 0) {
            return res.status(204).json([])
        }

        res.json(telemetries)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al solicitar las telemetrías de varios días" })
    }
}

export const updateTelemetry = async (req, res) => {
    const { device, telemetry } = req.body

    try {
        const day = new Date().toISOString().slice(0, 10)

        const result = await Telemetry.updateOne(
            {
                day,
                device,
                nsamples: { $lt: 86400 }
            },
            {
                $push: { telemetry },
                $min: { first: telemetry.ts },
                $max: { last: telemetry.ts },
                $inc: { nsamples: 1 }
            },
            { upsert: true }
        )

        res.json(result)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al guardar una telemetría" })
    }
}

export const getLastTelemetry = async (req, res) => {
    const { device } = req.params

    try {
        const data = await Telemetry.findOne(
            { device },
            { _id: 0, last: 1, telemetry: 1 },
            {
                sort: { telemetry: -1 }
            }
        )

        if (!data) {
            return res.status(404).json({ message: "No se encontraron telemetrías" })
        }

        const {last, telemetry} = data

        const lastTelemetry = telemetry.find( item => item.ts === last)
        
        res.json(lastTelemetry)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al solicitar la última telemetría" })
    }
}
```

## Crear el controlador en action.controller.js

1. Abrir el archivo `action.controller.js`.
2. Modificar el contenido del `action.controller.js`:

```
import { Action } from "../models/action.model.js"

export const getAllActions = async (req, res) => {
    const { device } = req.params

    try {
        const actions = await Action.find({ device })

        if (actions.length === 0) {
            return res.status(204).json([])
        }

        res.json(actions)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al solicitar todas las acciones" })
    }
}

export const getOneDayActions = async (req, res) => {
    const { device, day } = req.params

    try {
        const startDay = new Date(day)
        startDay.setUTCHours(0, 0, 0, 0)
        const endDay = new Date(day)
        endDay.setUTCHours(23, 59, 59, 999)

        const actions = await Action.find({
            device,
            ts: { $gte: startDay, $lte: endDay }
        })

        if (actions.length === 0) {
            return res.status(204).json([])
        }

        res.json(actions)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al solicitar las acciones de un día" })
    }
}

export const getFromToActions = async (req, res) => {
    const { device, from, to } = req.params

    try {
        const startDay = new Date(from)
        startDay.setUTCHours(0, 0, 0, 0)
        const endDay = new Date(to)
        endDay.setUTCHours(23, 59, 59, 999)

        const actions = await Action.find({
            device,
            ts: { $gte: startDay, $lte: endDay }
        })

        if (actions.length === 0) {
            return res.status(204).json([])
        }

        res.json(actions)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al solicitar las acciones de varios días" })
    }
}

export const createAction = async (req, res) => {
    const { device, command, parameter, user } = req.body

    try {
        const ts = new Date()

        const action = new Action({
            device,
            command,
            parameter,
            ts,
            user
        })

        const newAction = await action.save()
        res.json(newAction)
    } catch (error) {
        res.status(500).json({ message: error.message || "Error al guardar una acción" })
    }
}
```

## Modificar el entry point

1. Ver el sistema de archivos de la Raspberry Pi desde Visual Studio Code.
2. Abrir el archivo `index.js`.
3. Modificar el contenido del `index.js`:

```
import express from "express"
import mongoose from "mongoose"
import bodyParser from "body-parser"
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
        console.log("La API se conectó a MongoDB")
    } catch (error) {
        console.error(`Falló la conexión de la API a MongoDB: ${error}`)
    }
}

app.use(bodyParser.json())
app.use("/device", deviceRouter)
app.use("/telemetry", telemetryRouter)
app.use("/action", actionRouter)

connectToMongoDB()

app.listen(port, () => {
    console.log(`La API esta funcionando en el puerto ${port}`)
})
```
