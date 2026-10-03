# Especificación Oficial: Onion Architecture en Laravel

## 1. Los Cuatro Anillos de la Cebolla

### Anillo 1: Dominio (`Domain/`)
* **Propósito:** Contiene las entidades, value objects y reglas de negocio puras.
* **Regla:** Cero dependencias externas (`0` imports de `Illuminate\*`).
* **Componentes:** `Product`, `Sale`, `SaleItem`, `User`, `Category`, `Money`, `Quantity`, `Stock`, `Sku`, `Email`.

### Anillo 2: Aplicación (`Application/`)
* **Propósito:** Orquesta casos de uso del negocio.
* **Regla:** Depende exclusivamente de `Domain`.
* **Componentes:** `UseCases`, `DTOs`, `Ports/Inbound`, `Ports/Outbound`.
* **Transacciones:** Toda transacción atómica se ejecuta a través de `ITransactionManagerPort` sin importar `DB::transaction()` en el caso de uso.

### Anillo 3: Infraestructura (`Infrastructure/`)
* **Propósito:** Implementa los detalles técnicos y persistencia.
* **Regla:** Conoce la tecnología (Eloquent, MySQL, JWT, Bcrypt).
* **Componentes:** `Eloquent Models`, `Repositories`, `Mappers`, `LaravelTransactionManager`, `JwtTokenAdapter`.

### Anillo 4: Presentación (`Presentation/`)
* **Propósito:** Punto de entrada HTTP REST.
* **Regla:** PROHIBIDO importar `Infrastructure`.
* **Componentes:** `Controllers`, `Requests`, `Resources`, `ProblemDetails (RFC 7807)`, `Middlewares`.

### Composition Root (`Composition/`)
* **Propósito:** El pegamento arquitectónico.
* **Componentes:** `PortBindingsServiceProvider.php` (amarra interfaces de Application con clases concretas de Infrastructure).
