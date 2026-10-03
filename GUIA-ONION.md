# Guía Rápida de la Especificación Onion (Traducción Laravel + React)

Este documento permite entender rápidamente cómo está organizada la especificación de **Simple Stock Flow** y su traducción oficial a **Laravel / PHP 8.2+ y React**.

## En una frase

Aquí hay **dos versiones alternativas de la especificación del mismo sistema** Simple Stock Flow: una con backend de referencia en Python y otra en .NET. Ambas describen una **arquitectura hexagonal**; esta edición **traduce esas decisiones a Onion** para su implementación en React y Laravel. No son dos aplicaciones distintas ni código fuente listo para ejecutar: son documentos que explican qué debe hacer el sistema y cómo organizar su implementación.

```text
onion/
├── README.md                 # Alcance y contexto de esta edición
├── GUIA-ONION.md             # Este mapa de lectura rápida
├── spec-python/              # Especificación de referencia Python/FastAPI (Hexagonal)
│   └── adr/                  # Decisiones de arquitectura
└── spec-.net/                # Especificación de referencia .NET/ASP.NET Core (Hexagonal)
    └── adr/                  # Decisiones de arquitectura
```

## Qué significa Onion aquí

Onion describe la **organización interna del backend** y la dirección de sus dependencias. Las capas externas pueden usar las internas; el Dominio, que está en el centro, no conoce las capas externas.

```text
Presentación (Anillo 4) → Aplicación (Anillo 2) → Dominio (Anillo 1 - Centro)
Infraestructura (Anillo 3) → Aplicación y Dominio
Bootstrap conecta las implementaciones externas con sus interfaces (Composition Root)
```

### 1. Dominio — el centro (Anillo 1 · PHP puro)
Contiene el modelo del negocio: entidades (`Product`, `Sale`, `User`), objetos de valor (`Money` con `BigDecimal`, `Quantity`, `Stock`, `Username`, `Role`) y reglas invariantes que siempre deben cumplirse. **No debe depender de Laravel, base de datos ni ORM**.

### 2. Aplicación — los casos de uso (Anillo 2 · solo importa Domain)
Contiene las operaciones que el sistema ofrece (`PlaceSaleService`, `ProductCatalogService`, `AuthenticationService`, `GetSalesService`, `SalesReportService`). Define los **5 puertos de entrada** (`Ports/Inbound/`) y los **10 puertos de salida** (`Ports/Outbound/`).

### 3. Infraestructura — los detalles técnicos (Anillo 3)
Implementa las interfaces que requiere Aplicación: repositorios Eloquent, migraciones de base de datos, `LaravelUnitOfWork` (`DB::transaction()`), generación de tokens JWT y hashing Argon2.

### 4. Presentación — la entrada y salida HTTP (Anillo 4 · Prohibido importar Infra)
Recibe solicitudes HTTP, valida forma y traduce respuestas y excepciones de negocio al estándar **RFC 7807 Problem Details** (422, 409, 401, 404).

### 5. Bootstrap — el punto de ensamblaje (No es anillo)
`app/Bootstrap/PortBindingsServiceProvider.php`: Configura el Service Container y conecta cada interfaz con su implementación concreta.

---

## Cómo leer cada especificación

| Archivo o carpeta | Para qué sirve |
|---|---|
| `constitution.md` | Principios obligatorios y cómo verificar que se respetan (DIP, pureza de capas). |
| `spec.md` | Qué debe hacer el producto: actores, historias, criterios de aceptación (CA) y reglas de negocio (RN). |
| `architecture.md` | Capas, responsabilidades, topología y flujo entre las partes. |
| `data-model.md` | Entidades, campos, relaciones, restricciones (9 CHECK a mano) y reglas de persistencia. |
| `api-contract.md` | Rutas de la API, autorización, solicitudes, respuestas y errores. |
| `tasks.md` | Tareas de desarrollo mapeadas 1 a 1 con los casos de uso (T-04, T-06, T-07, T-08, T-10). |
| `adr/` | Registros de decisiones de arquitectura (ADR-001 a ADR-010). |
