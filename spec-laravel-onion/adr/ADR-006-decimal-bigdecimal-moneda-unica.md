# ADR-006: Manejo de Moneda Única y Precisión Decimal con BigDecimal

## Estado
Aceptado

## Contexto
El spec exige que los importes monetarios sean exactos, sin float, monomoneda ('COP'), con escala 2 fija y redondeo `HALF_UP` (D-05, D-07).

## Decisión
1. Se utiliza `Brick\Math\BigDecimal` en el Value Object `Money` (o en su defecto `bcmath` puro con escala 2 fija).
2. El sistema opera exclusivamente en 'COP'.
3. El mapper de infraestructura rechaza cualquier valor que contenga más de 2 decimales en lugar de redondear silenciosamente.

## Consecuencias
- **Positivas:** Cero errores de punto flotante en cálculo de totales y reportes contables.
