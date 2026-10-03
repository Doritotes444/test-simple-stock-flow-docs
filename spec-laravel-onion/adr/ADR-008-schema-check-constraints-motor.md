# ADR-008: Definición Explícita de los 9 CHECK Constraints en MySQL

## Estado
Aceptado

## Contexto
El schema builder estándar de Laravel y los generadores de ORM no autocomparan ni aplican restricciones CHECK de SQL en motores relacionales si no se declaran explícitamente (`data-model.md` §4, RN-01, RN-02, RN-03, RN-10, RN-11).

## Decisión
1. Las migraciones en `database/migrations/` declaran explícitamente las 9 restricciones `CHECK` en el motor MySQL 8.4:
   - `products.stock >= 0`
   - `products.price > 0`
   - `products.version >= 0`
   - `sale_items.quantity > 0`
   - `sale_items.unit_price > 0`
   - etc.

## Consecuencias
- **Positivas:** La integridad de los datos está blindada a nivel de motor de base de datos además del dominio.
