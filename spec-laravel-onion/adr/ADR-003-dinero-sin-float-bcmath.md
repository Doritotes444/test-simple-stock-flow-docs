# ADR-003: Representación de Dinero sin Float mediante BCMath

## Estado
Aceptado

## Contexto
El tipo nativo `float` en PHP tiene imprecisiones de punto flotante binario (IEEE 754), lo que genera pérdida o acumulación errónea de centavos en ventas y reportes contables.

## Decisión
1. Se prohíbe el uso de `float` primitivo en cálculos monetarios del Dominio (D-05, D-07).
2. El Value Object `Money` almacena el valor como `string` con escala decimal fija de 2 posiciones y utiliza la extensión nativa `bcmath` (`bcadd`, `bcmul`, `bccomp`) con redondeo `HALF_UP`.
3. En la base de datos se almacena como `DECIMAL(12, 2)`.

## Consecuencias
- **Positivas:** Precisión matemática exacta, reportes consistentes y cumplimiento de la especificación financiera.
