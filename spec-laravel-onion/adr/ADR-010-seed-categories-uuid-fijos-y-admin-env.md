# ADR-010: Categorías con UUIDs Fijos y Creación de Admin desde Variables de Entorno

## Estado
Aceptado

## Contexto
El plan de datos (D-10, `data-model.md` §9) prohíbe sembrar contraseñas o usuarios en scripts SQL planos para evitar comprometer seguridad y credenciales en repositorios públicos.

## Decisión
1. Las 5 categorías iniciales se insertan mediante la migración `seed_categories` con UUIDs literales fijos como datos de referencia inmutables.
2. El usuario administrador inicial no se siembra desde SQL; se genera al iniciar la aplicación leyendo las variables de entorno `ADMIN_EMAIL` y `ADMIN_PASSWORD` y aplicando hashing `Argon2` / `Bcrypt`.

## Consecuencias
- **Positivas:** Seguridad por diseño, respeto a D-10 y cero secretos en Git.
