# ADR-001: Motor MySQL Vacío y Propiedad de Esquema en API

## Estado
Aceptado

## Contexto
El contenedor `db` en Docker Compose necesita iniciar con MySQL 8.4. Existe el riesgo de que los desarrolladores inserten scripts DDL manuales (`schema.sql`) dentro de la carpeta `infra/`.

## Decisión
1. `infra/docker-compose.yml` inicia el servicio MySQL con el motor completamente vacío.
2. El repositorio `test-simple-stock-flow-api` es el único dueño y responsable del esquema mediante migraciones de Laravel (`php artisan migrate`).

## Consecuencias
- **Positivas:** Control de versiones del esquema en código de backend, reproducibilidad y cumplimiento del Artículo V.
- **Negativas:** La API debe esperar a que MySQL esté saludable (`service_healthy`) antes de ejecutar migraciones.
