# Springboot-API-REST (Ventas)

Microservicio Spring Boot que expone una API REST para gestionar ventas.

Base path: `/api/v1/ventas`

## Contenido
- Descripción
- Requisitos
- Ejecutar localmente (Maven)
- Ejecutar con Docker
- Entidad Venta (modelo)
- Endpoints
- Ejemplos curl
- Notas

## Descripción

Este servicio permite crear, listar, obtener, actualizar y eliminar recursos `Venta`.

## Requisitos
- Java 17+
- Maven (opcional si usa `mvnw`)
- Docker (opcional)

El proyecto incluye `mvnw` y `mvnw.cmd` para ejecutar Maven sin instalación global.

## Ejecutar localmente

Desde la carpeta del proyecto:

```bash
./mvnw spring-boot:run
```

o con Maven instalado:

```bash
mvn spring-boot:run
```

La aplicación se expondrá en el puerto configurado en `src/main/resources/application.properties` (por defecto suele ser 8080).

## Ejecutar con Docker

Construir y ejecutar la imagen desde la carpeta con el `Dockerfile`:

```bash
docker build -t ventas-api:latest .
docker run --rm -p 8080:8080 ventas-api:latest
```

## Entidad Venta

Campos del recurso `Venta`:

- idVenta: Long (autogenerado)
- direccionCompra: String (obligatoria)
- valorCompra: int
- fechaCompra: LocalDate (obligatoria, formato ISO yyyy-MM-dd)
- despachoGenerado: Boolean (indica si se generó despacho)

Ejemplo JSON de una venta:

```json
{
  "direccionCompra": "Calle Falsa 123",
  "valorCompra": 25000,
  "fechaCompra": "2026-05-18",
  "despachoGenerado": false
}
```

## Endpoints

- POST /api/v1/ventas
  - Crear una nueva venta.
  - Request body: JSON de `Venta` (sin `idVenta`).
  - Response: 201 Created con Location header y el objeto creado.

- GET /api/v1/ventas
  - Obtener todas las ventas.
  - Response: 200 OK con un array de `Venta`.

- GET /api/v1/ventas/{idVenta}
  - Obtener una venta por id.
  - Response: 200 OK o 404 si no existe.

- PUT /api/v1/ventas/{idVenta}
  - Actualizar una venta existente.
  - Request body: JSON con los nuevos valores.
  - Response: 200 OK con el objeto actualizado.

- DELETE /api/v1/ventas/{idVenta}
  - Eliminar una venta.
  - Response: 204 No Content si se elimina correctamente.

## Ejemplos (curl)

# Crear venta
curl -X POST http://localhost:8080/api/v1/ventas \
  -H "Content-Type: application/json" \
  -d '{"direccionCompra":"Calle Falsa 123","valorCompra":25000,"fechaCompra":"2026-05-18","despachoGenerado":false}'

# Listar ventas
curl http://localhost:8080/api/v1/ventas

# Obtener por id
curl http://localhost:8080/api/v1/ventas/1

# Actualizar venta
curl -X PUT http://localhost:8080/api/v1/ventas/1 \
  -H "Content-Type: application/json" \
  -d '{"direccionCompra":"Av Siempreviva 742","valorCompra":25500,"fechaCompra":"2026-05-19","despachoGenerado":true}'

# Eliminar venta
curl -X DELETE http://localhost:8080/api/v1/ventas/1