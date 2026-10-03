# Especificación General y Documentación - Simple Stock Flow

## 1. Mapeo de Arquitectura
Este repositorio contiene las especificaciones de referencia y la traducción oficial de la arquitectura del sistema **Simple Stock Flow** hacia **Arquitectura Onion en Laravel / PHP 8.2+**.

```
simple-stock-flow/
├── test-simple-stock-flow-docs/     <-- Especificación y Mapeo SDD
├── test-simple-stock-flow-api/      <-- Backend Laravel (Onion 4 Capas)
├── test-simple-stock-flow-app/      <-- Frontend React SPA
├── test-simple-stock-flow-page/     <-- Landing comercial estática (HTML/CSS/JS)
├── test-simple-stock-flow-infra/    <-- Docker Compose & MySQL 8.4
└── test-simple-stock-flow-tool/     <-- CLI Seeder HTTP
```

## 2. Documentos de Referencia
- `spec-laravel-onion/architecture-onion.md`: Definición formal de los 4 anillos (`Domain`, `Application`, `Infrastructure`, `Presentation`) y el Composition Root.
- `spec-laravel-onion/mapping-sdd.md`: Mapeo conceptual de Puertos/Adaptadores y SDD a la estructura de carpetas de Laravel.
