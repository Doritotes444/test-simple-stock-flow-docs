# Mapeo SDD: De Arquitectura Hexagonal a Onion en Laravel

## 1. Tabla de Equivalencias Arquitectónicas

| Concepto Hexagonal (Puertos y Adaptadores) | Capa en Onion Architecture | Ubicación en Laravel (`api`) |
| :--- | :--- | :--- |
| **Core Domain Model** | Anillo 1: `Domain/` | `app/Domain/Model/` & `app/Domain/ValueObject/` |
| **Domain Services** | Anillo 1: `Domain/` | `app/Domain/Service/` |
| **Driving Ports (Puertos de Entrada)** | Anillo 2: `Application/` | `app/Application/Ports/Inbound/` |
| **Driven Ports (Puertos de Salida)** | Anillo 2: `Application/` | `app/Application/Ports/Outbound/` |
| **Use Cases (Lógica de Aplicación)** | Anillo 2: `Application/` | `app/Application/UseCase/` |
| **Driven Adapters (Persistencia DB, Auth)**| Anillo 3: `Infrastructure/`| `app/Infrastructure/Persistence/` |
| **Driving Adapters (Controladores HTTP REST)**| Anillo 4: `Presentation/`| `app/Presentation/Http/Controller/` |
| **IoC / Dependency Injection Wiring** | Composition Root | `app/Composition/PortBindingsServiceProvider.php` |

## 2. Reglas no Negociables
1. `version` (concurrencia optimista) reside exclusivamente en SQL/Infrastructure; `Domain` no contiene propiedades de versión técnica.
2. `total` y `subtotal` son derivados del cálculo de ítems (`unit_price * quantity`) según el Artículo VII.
3. Las migraciones viven en `api` (ADR-001); `infra` solo provee el motor MySQL vacío.
