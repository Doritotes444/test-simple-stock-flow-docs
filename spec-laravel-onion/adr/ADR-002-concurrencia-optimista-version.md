# ADR-002: Concurrencia Optimista, Versionado en Persistencia y Reintentos

## Estado
Aceptado

## Contexto
En un entorno con múltiples cajeros vendiendo productos simultáneamente, pueden ocurrir sobreventas si no se controla la concurrencia.

## Decisión
1. La columna `version` se maneja exclusivamente en la tabla de persistencia SQL dentro de `Infrastructure`.
2. La entidad de `Domain/Model/Product.php` no contiene la propiedad técnica `$version`.
3. `EloquentProductRepository` actualiza con `UPDATE products SET stock = ?, version = version + 1 WHERE id = ? AND version = ?`. Si las filas afectadas son 0, lanza `ProductConcurrencyException`.
4. `PlaceSaleUseCase` en `Application` captura `ProductConcurrencyException` y reintenta la transacción hasta 3 veces con backoff antes de devolver HTTP 409 Conflict.

## Consecuencias
- **Positivas:** Previene sobreventas, pureza total en `Domain` y experiencia fluida para el usuario.
