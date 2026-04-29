---
name: systematic-debugging
description: Debug Django bugs and test failures by finding root cause first. Use when errors, failing tests, or unexpected behavior appear.
---

# Systematic Debugging

No aplicar parches a ciegas. Primero entender la causa.

## Flujo

1. Reproducir el error.
2. Leer el mensaje completo y stack trace.
3. Revisar cambios recientes con `git diff`.
4. Seguir el flujo de datos hasta encontrar la causa.
5. Corregir el punto correcto.
6. Agregar o actualizar test si el bug es critico o puede volver.
7. Correr tests relevantes.

## Comandos utiles

```bash
python manage.py test
pytest
python manage.py showmigrations
python manage.py migrate
```

Con Docker Compose:

```bash
docker compose logs --tail=100 web
docker compose exec web python manage.py test
docker compose exec web pytest
```

## Django

- Formularios: revisar `form.is_valid()` y `form.errors`.
- Permisos: probar usuario anonimo, usuario normal y usuario autorizado.
- Queries: buscar N+1 cuando hay listas con relaciones.
- Migraciones: revisar `showmigrations` antes de tocar migraciones antiguas.
- HTMX: revisar `HX-Request` solo si el proyecto usa HTMX.

## Cuando detenerse

Si despues de varios intentos no hay causa clara, parar y explicar:

- que se probo;
- que se sabe;
- que falta revisar.
