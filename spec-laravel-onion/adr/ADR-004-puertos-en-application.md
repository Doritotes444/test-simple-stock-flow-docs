# ADR-004: Ubicación de los Puertos en Application (Artículo IV - DIP)

## Estado
Aceptado

## Contexto
En la arquitectura Onion clásica (Palermo), los contratos de repositorios a veces se dibujan dentro de `Domain`. Sin embargo, la Constitución del proyecto (Artículo IV) establece que los puertos son contratos de los Casos de Uso.

## Decisión
1. Los puertos de entrada (`Ports/Inbound/`) y de salida (`Ports/Outbound/`) se ubican estrictamente en `app/Application/Ports/`.
2. El Dominio (`app/Domain/`) se enfoca exclusivamente en la verdad del negocio: entidades, Value Objects, servicios de dominio y excepciones.
3. El pegamento entre los puertos de `Application` y los adaptadores de `Infrastructure` se delega al `Composition Root` (`PortBindingsServiceProvider.php`).

## Consecuencias
- **Positivas:** Cumplimiento del Artículo IV y verificabilidad inmediata mediante análisis estático.
