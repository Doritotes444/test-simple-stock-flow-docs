# ADR-009: Reintentos de Concurrencia Optimista (Máx 3) y Código 409

## Estado
Aceptado

## Contexto
El criterio de aceptación CA-04.6 y `api-contract.md` E-10 exigen que las colisiones concurrentes en el stock de productos no fallen inmediatamente, sino que permitan reintentar la operación antes de emitir un conflicto.

## Decisión
1. `PlaceSaleService` ejecuta un bucle de reintento de hasta 3 intentos ante `ConcurrencyConflict`.
2. Si tras 3 intentos la colisión persiste, se emite una excepción de aplicación que `Presentation` traduce al RFC 7807 con código HTTP 409 Conflict.

## Consecuencias
- **Positivas:** Cumplimiento estricto del criterio de aceptación CA-04.6.
