# TP-12 — Sistema de Reservas de Salas con CRUD Web y MongoDB

## Descripción

Aplicación web desarrollada con Node.js, Express, EJS, MongoDB Atlas y Mongoose para gestionar reservas de salas.

Este trabajo práctico continúa el sistema desarrollado en las unidades anteriores e incorpora la actualización parcial del estado y la eliminación de reservas con confirmación desde la interfaz web.

La aplicación permite crear, listar, consultar, editar y eliminar reservas, almacenando los datos de manera persistente en MongoDB Atlas.

También dispone de una API REST para consultar y administrar las reservas mediante solicitudes HTTP y respuestas JSON.

## Tecnologías utilizadas

- Node.js
- JavaScript
- Express.js
- MongoDB Atlas
- Mongoose
- EJS
- express-ejs-layouts
- Morgan
- Postman
- ESLint
- Prettier

## Estructura del proyecto

El proyecto mantiene la organización modular de las unidades anteriores:

- `src/config/`: configuración del entorno y conexión a MongoDB.
- `src/controladores/`: controladores de la aplicación web y de la API.
- `src/middleware/`: validaciones, seguimiento de solicitudes y manejo de errores.
- `src/modelos/`: modelo de reserva de Mongoose.
- `src/rutas/`: rutas web y rutas de la API.
- `src/servicios/`: operaciones sobre las reservas almacenadas en MongoDB.
- `views/`: plantillas EJS de la interfaz web.
- `public/`: archivos estáticos.
- `scripts/`: scripts auxiliares.

## Instalación y configuración

1. Clonar o descargar el repositorio.
2. Abrir una terminal en la carpeta del proyecto.
3. Instalar las dependencias:

```bash
npm ci
```

4. Crear el archivo `.env` en la raíz del proyecto, tomando como referencia `.env.example`.
5. Configurar la variable `MONGODB_URI` con la conexión a MongoDB Atlas.

La base de datos prevista para este trabajo práctico es `reservas_salas_u12` y la colección es `reservas`.

El archivo `.env` contiene información privada y no debe incluirse en el repositorio.

## Ejecución

Iniciar el servidor con:

```bash
node --env-file=.env src/index.js
```

La aplicación estará disponible en:

`http://localhost:3000`

## Funcionalidades de la aplicación web

### Crear reservas

Permite registrar una nueva reserva desde un formulario web.

Cada reserva contiene los siguientes campos:

- Estudiante
- Email
- Sala
- Fecha
- Turno
- Cantidad de personas
- Estado

### Listar reservas

Muestra las reservas almacenadas en MongoDB.

Ruta:

`GET /reservas`

### Consultar una reserva

Permite visualizar los datos de una reserva mediante su identificador.

Ruta:

`GET /reservas/:id`

### Editar una reserva

Permite modificar todos los campos de una reserva existente desde la interfaz web.

Ruta para mostrar el formulario:

`GET /reservas/:id/editar`

Ruta para guardar los cambios:

`POST /reservas/:id/editar`

El servidor valida los datos recibidos y utiliza el servicio MongoDB para reemplazar la reserva mediante `findOneAndReplace()`, con las validaciones de Mongoose activadas.

Cuando la operación finaliza correctamente, el servidor responde con HTTP 303 y redirige al detalle de la reserva.

El identificador original se conserva.

### Cambiar parcialmente el estado de una reserva

El TP12 incorpora un formulario específico para modificar únicamente el estado de una reserva.

Ruta para mostrar el formulario:

`GET /reservas/:id/estado`

Ruta para guardar el cambio:

`POST /reservas/:id/estado`

El formulario permite seleccionar uno de los estados admitidos:

- `pendiente`
- `confirmada`

El servidor valida que la solicitud contenga únicamente el campo `estado`.

La actualización se realiza mediante `findOneAndUpdate()`, utilizando el servicio MongoDB existente.

Después de guardar correctamente, el servidor responde con HTTP 303 y redirige al detalle de la reserva.

Los demás campos permanecen sin modificaciones.

### Eliminar una reserva

El TP12 incorpora la eliminación de reservas desde la interfaz web mediante una pantalla de confirmación.

Ruta para mostrar la confirmación:

`GET /reservas/:id/eliminar`

Ruta para ejecutar la eliminación:

`POST /reservas/:id/eliminar`

La pantalla muestra los datos de la reserva y permite confirmar o cancelar la operación.

- **Cancelar:** regresa al detalle sin eliminar la reserva.
- **Confirmar eliminación:** ejecuta la operación mediante `deleteOne()` y redirige al listado con HTTP 303.

La eliminación es definitiva.

Si la reserva no existe, la aplicación devuelve HTTP 404.

## Validaciones

La aplicación comprueba que:

- El estudiante no esté vacío.
- El email contenga `@`.
- La sala sea Norte, Sur o Multimedia.
- La fecha no esté vacía.
- El turno sea Mañana, Tarde o Noche.
- La cantidad de personas sea un número entero entre 1 y 6.
- El estado sea `pendiente` o `confirmada`.
- No se reciban campos adicionales en el reemplazo web.
- El cambio parcial contenga únicamente un estado válido.

Las solicitudes de edición o actualización parcial inválidas devuelven HTTP 400.

Los identificadores inválidos se rechazan y las reservas inexistentes se informan mediante HTTP 404.

## API REST

La aplicación conserva las operaciones desarrolladas en las unidades anteriores.

| Método | Ruta | Descripción |
|---|---|---|
| GET | `/api/reservas` | Consultar reservas |
| GET | `/api/reservas/:id` | Consultar una reserva |
| POST | `/api/reservas` | Crear una reserva |
| PUT | `/api/reservas/:id` | Reemplazar una reserva |
| PATCH | `/api/reservas/:id` | Actualizar parcialmente |
| DELETE | `/api/reservas/:id` | Eliminar una reserva |

La API permite consultar reservas mediante filtros por sala, ordenamiento y paginación.

Las respuestas de la API utilizan JSON.

## Pruebas realizadas

Se realizaron pruebas utilizando el navegador y Postman durante el desarrollo del sistema.

Resultados comprobados:

- Conexión exitosa con MongoDB Atlas.
- Creación de reservas desde la web.
- Visualización del listado y del detalle.
- Edición completa de reservas.
- Redirección HTTP 303 después de guardar cambios.
- Modificación parcial del estado desde la web.
- Conservación de los demás campos después del cambio de estado.
- Rechazo HTTP 400 de solicitudes inválidas.
- Visualización de la pantalla de confirmación de eliminación.
- Cancelación de eliminación sin modificar la reserva.
- Eliminación definitiva de una reserva.
- Redirección al listado después de eliminar.
- Respuesta HTTP 404 al consultar una reserva eliminada.
- Respuesta HTTP 404 al intentar acceder a la confirmación de eliminación de una reserva inexistente.

## Verificación del código

Para comprobar el formato y las reglas de calidad:

```bash
npm run check
```

Se ejecutó correctamente la verificación de Prettier y ESLint, sin errores.

## Conclusión

El TP12 completa las operaciones CRUD desde la interfaz web del sistema de reservas.

La aplicación permite crear, consultar, editar, actualizar parcialmente y eliminar reservas, reutilizando los servicios de MongoDB y manteniendo las validaciones existentes.

La eliminación incorpora una pantalla de confirmación para evitar operaciones accidentales.

El sistema conserva su API REST y almacena los datos de forma persistente en MongoDB Atlas.