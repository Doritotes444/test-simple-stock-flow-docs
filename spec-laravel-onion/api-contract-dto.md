# Contrato DTO Congelado Campo por Campo

Este documento define la fuente de verdad inmutable para la serialización de datos entre la API (Laravel) y el Frontend (React), coincidiendo con `src/infrastructure/http/dto/api.dto.ts`.

## 1. Reglas de Serialización
- **Nombres de campos:** `camelCase` en JSON.
- **Fechas:** Formato ISO 8601 UTC (`YYYY-MM-DDTHH:mm:ssZ` o `+00:00`).
- **Moneda:** Valores numéricos con 2 decimales para `price` / `total` / `subtotal`, y código de moneda siempre presente (`currency: "COP"`).
- **URLs de imágenes:** Nunca nulas ni filtradas (`imageUrl`).

---

## 2. DTOs de Entrada y Salida

### 2.1 Autenticación
`POST /api/auth/login`
```json
// Request
{
  "username": "cajero_01",
  "password": "string(min:6)"
}

// Response 200 OK
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "uuid-v4",
    "username": "cajero_01",
    "role": "seller"
  }
}
```

### 2.2 Productos
`POST /api/products`
```json
// Request
{
  "sku": "PROD-001",
  "name": "Arroz Diana 1kg",
  "description": "Arroz blanco",
  "price": 4500.00,
  "stock": 50,
  "categoryId": "uuid-v4-category"
}

// Response 201 Created
{
  "id": "uuid-v4",
  "sku": "PROD-001",
  "name": "Arroz Diana 1kg",
  "description": "Arroz blanco",
  "price": 4500.00,
  "currency": "COP",
  "stock": 50,
  "categoryId": "uuid-v4-category",
  "isActive": true,
  "imageUrl": "https://..."
}
```

### 2.3 Registro de Venta
`POST /api/sales`
```json
// Request
{
  "userId": "uuid-v4-user",
  "items": [
    {
      "productId": "uuid-v4-product",
      "quantity": 2
    }
  ]
}

// Response 201 Created
{
  "id": "uuid-v4-sale",
  "userId": "uuid-v4-user",
  "total": 9000.00,
  "currency": "COP",
  "createdAt": "2026-10-03T10:00:00Z",
  "items": [
    {
      "id": "uuid-v4-item",
      "productId": "uuid-v4-product",
      "productName": "Arroz Diana 1kg",
      "quantity": 2,
      "unitPrice": 4500.00,
      "subtotal": 9000.00
    }
  ]
}
```

### 2.4 Reporte de Ventas
`GET /api/reports/sales?startDate=...&endDate=...`
```json
// Response 200 OK
{
  "totalTransactions": 15,
  "totalRevenue": 245000.00,
  "currency": "COP",
  "totalPages": 1,
  "sales": [
    {
      "id": "uuid-v4-sale",
      "userId": "uuid-v4-user",
      "total": 9000.00,
      "currency": "COP",
      "createdAt": "2026-10-03T10:00:00Z",
      "items": [...]
    }
  ]
}
```

---

## 3. Cuerpos de Error RFC 7807 (Problem Details)

### 3.1 Error de Validación (HTTP 422)
```json
{
  "type": "https://tools.ietf.org/html/rfc7807",
  "title": "Error de Validación",
  "status": 422,
  "detail": "Los datos enviados contienen errores de validación",
  "invalidParams": [
    {
      "name": "sku",
      "reason": "El código SKU es obligatorio y debe tener al menos 3 caracteres"
    }
  ]
}
```

### 3.2 Conflicto de Concurrencia (HTTP 409)
```json
{
  "type": "https://tools.ietf.org/html/rfc7807",
  "title": "Conflicto de Concurrencia",
  "status": 409,
  "detail": "El stock del producto cambió durante la transacción tras 3 intentos. Por favor recargue el inventario."
}
```

### 3.3 No Autorizado (HTTP 401)
```json
{
  "type": "https://tools.ietf.org/html/rfc7807",
  "title": "No Autorizado",
  "status": 401,
  "detail": "Credenciales inválidas o token expirado"
}
```
