# ADR-007: Puerto UnitOfWork con Callback

## Estado
Aceptado

## Contexto
El Artículo II de la Constitución nombra el puerto de transacción como `UnitOfWork`. Se requiere evitar llamadas manuales `begin/commit/rollback` que puedan dejar transacciones abiertas.

## Decisión
1. El puerto `UnitOfWork` expone el método `run(Closure $operation): mixed`.
2. La implementación en `Infrastructure/Persistence/LaravelUnitOfWork.php` delega la ejecución a `DB::transaction($operation)`.

## Consecuencias
- **Positivas:** `Application` no importa `Illuminate\Support\Facades\DB` y los rollbacks son 100% automáticos ante excepciones.
