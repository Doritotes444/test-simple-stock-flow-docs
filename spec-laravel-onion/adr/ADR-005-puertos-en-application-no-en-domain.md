# ADR-005: Ubicación de Puertos en Application y no en Domain

## Estado
Aceptado

## Contexto
En la arquitectura Onion clásica (Palermo), los puertos de repositorio a veces se dibujan dentro de `Domain`. Sin embargo, la Constitución de este proyecto (Artículos II y IV - DIP) establece que los puertos son contratos definidos por quien los consume: la capa de Aplicación.

## Decisión
1. Los 5 puertos de entrada (`Ports/Inbound/`) y los 10 puertos de salida (`Ports/Outbound/`) se ubican estrictamente en `app/Application/Ports/`.
2. El Dominio (`app/Domain/`) contiene exclusivamente la lógica de negocio pura: entidades, Value Objects y excepciones de negocio.
3. El ensamblaje de puertos con adaptadores se realiza en `app/Bootstrap/PortBindingsServiceProvider.php`.

## Consecuencias
- **Positivas:** Cumplimiento del Artículo IV (DIP) y verificabilidad mediante análisis estático.
