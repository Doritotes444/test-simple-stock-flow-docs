# Plan de Trabajo por Fases (Laravel + React)

## Estrategia de Ramas y Commits
Todas las fases se desarrollan en ramas cortas con PR hacia `main`. El commit de documentación es siempre el primero de cada rama.

```
main                        ← siempre revisable por el instructor
 └─ docs/onion-decision     ← P0, SOLO documentación
 └─ phase/1-domain
 └─ phase/2-schema-infra
 └─ phase/3-use-cases
 └─ phase/4-robustness
 └─ phase/5-app
 └─ phase/6-satellites
 └─ phase/7-delivery
```

## Fases de Implementación

### Fase 0: Documentación Base y Decisión Onion (`docs/onion-decision`)
- `GUIA-ONION.md`, `spec-laravel-onion/architecture-onion.md`, ADR-001 a ADR-010.
- Contrato DTO congelado campo por campo.

### Fase 1: Dominio Puro (`phase/1-domain`)
- Entidades: `Product`, `Sale`, `SaleItem`, `Category`, `User`.
- Value Objects: `Money` (`BigDecimal`), `Quantity`, `Username`, `Role`, IDs.
- Excepciones de negocio en español (`BusinessRuleViolation`).
- Verificación: Test de carga sin Laravel (`grep -R "Illuminate" app/Domain` = 0).

### Fase 2: Esquema e Infraestructura (`phase/2-schema-infra`)
- Migraciones con 9 `CHECK constraints` manuales y `seed_categories` con UUIDs fijos.
- Modelos Eloquent (`Infrastructure/Persistence/Model/`).
- Mappers bidireccionales (`version` encapsulada en persistencia).
- `LaravelUnitOfWork` implementando el puerto `UnitOfWork`.

### Fase 3: Casos de Uso (`phase/3-use-cases`)
- 5 Puertos Inbound + 10 Puertos Outbound.
- Servicios de aplicación: `PlaceSaleService` (T-10), `ProductCatalogService` (T-04), `GetSalesService` (T-07), `SalesReportService` (T-08), `AuthenticationService` (T-06).

### Fase 4: Robustez y API REST (`phase/4-robustness`)
- Controladores y Form Requests (validación de forma únicamente).
- Respuestas RFC 7807 Problem Details (422, 409, 401, 404).
- Reintentos de concurrencia optimista (máx 3).
- `Bootstrap/PortBindingsServiceProvider.php`.

### Fase 5: Frontend React (`phase/5-app`)
- Estructura `domain/`, `application/`, `infrastructure/` (con `dto/api.dto.ts`), `features/`.
- UI de Catálogo, Carrito/POS, Reportes y Autenticación.

### Fase 6: Satélites (`phase/6-satellites`)
- `page`: Landing page comercial HTML/CSS pura.
- `tool`: CLI Seeder vía consumo de endpoints HTTP REST.
- `infra`: Docker Compose con MySQL 8.4 y volúmenes nombrados.

### Fase 7: Entrega y Verificación (`phase/7-delivery`)
- Ejecución de `verify.sh` dentro del contenedor.
- Validación de los 13 puntos de verificación técnica.
