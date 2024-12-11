# Capítulo 12: Rutas en Express

## Crear un archivo JS para cada grupo de rutas

1. Abrir la terminal de la Raspberry Pi desde Visual Studio Code.
2. Ejecutar `cd proyecto-raspberrypi-iot`.
3. Ejecutar `cd api`.
4. Ejecutar `mkdir routes`.
5. Ejecutar `cd routes`.
6. Ejecutar `touch device.routes.js`.
7. Ejecutar `touch telemetry.routes.js`.
8. Ejecutar `touch action.routes.js`.

## Crear las rutas en device.routes.js

1. Ver el sistema de archivos de la Raspberry Pi desde Visual Studio Code.
2. Abrir el archivo `device.routes.js`.
3. Modificar el contenido del `device.routes.js`:

```
import { Router } from "express"
import { getAllDevices, getOneDevice, createDevice } from "../controllers/device.controller.js"

export const deviceRouter = Router()

deviceRouter.get("/", getAllDevices)
deviceRouter.get("/:name", getOneDevice)
deviceRouter.post("/", createDevice)
```

## Crear las rutas en telemetry.routes.js

1. Abrir el archivo `telemetry.routes.js`.
2. Modificar el contenido del `telemetry.routes.js`:

```
import { Router } from "express"
import { getLastTelemetry, getAllTelemetries, getOneDayTelemetries, getFromToTelemetries, updateTelemetry } from "../controllers/telemetry.controller.js"

export const telemetryRouter = Router()

telemetryRouter.get("/last/:device", getLastTelemetry)
telemetryRouter.get("/:device", getAllTelemetries)
telemetryRouter.get("/:device/:day", getOneDayTelemetries)
telemetryRouter.get("/:device/:from/:to", getFromToTelemetries)
telemetryRouter.put("/", updateTelemetry)
```

## Crear las rutas en action.routes.js

1. Abrir el archivo `action.routes.js`.
2. Modificar el contenido del `action.routes.js`:

```
import { Router } from "express"
import { getAllActions, getOneDayActions, getFromToActions, createAction } from "../controllers/action.controller.js"

export const actionRouter = Router()

actionRouter.get("/:device", getAllActions)
actionRouter.get("/:device/:day", getOneDayActions)
actionRouter.get("/:device/:from/:to", getFromToActions)
actionRouter.post("/", createAction)
```

## Modificar el entry point

1. Abrir el archivo `index.js`.
2. Modificar el contenido del `index.js`:

```
import express from "express"
import { deviceRouter } from "./routes/device.routes.js"
import { telemetryRouter } from "./routes/telemetry.routes.js"
import { actionRouter } from "./routes/action.routes.js"

const app = express()
const port = 3000

app.use("/device", deviceRouter)
app.use("/telemetry", telemetryRouter)
app.use("/action", actionRouter)

app.listen(port, () => { 
    console.log(`La API esta funcionando en el puerto ${port}`)
})
```
