---
name: code-reviewer
description: Review Django changes with a practical checklist. Use after meaningful code changes, especially critical flows, permissions, models, migrations, payments, reports, integrations, or bug fixes.
---

# Code Reviewer

Revisa cambios de Django con foco pragmatico. El objetivo es encontrar riesgos reales, no imponer herramientas que el proyecto no usa.

## Como revisar

1. Leer `git diff` y los archivos modificados.
2. Priorizar bugs, regresiones, seguridad, datos, permisos y migraciones.
3. Verificar que cambios criticos tengan tests.
4. Revisar que se haya corrido `migrate` antes de localhost.
5. Revisar que el servidor pueda quedar levantado para prueba local.

## Checklist

### Critico

- Permisos, login y datos sensibles correctos.
- No hay secretos hardcodeados.
- Formularios validan en backend.
- Cambios de modelos tienen migracion.
- Migraciones no rompen datos existentes.
- En proyectos nuevos, Django Admin esta disponible para el superusuario.
- Pagos, calculos, reportes e integraciones tienen tests relevantes.
- Errores importantes no se silencian con `except: pass`.

### Django

- Vistas usan metodos HTTP correctos.
- Formularios muestran errores al usuario.
- Consultas con relaciones usan `select_related()` o `prefetch_related()` cuando corresponde.
- Templates mantienen el patron existente del proyecto.
- HTMX solo se exige si el proyecto ya lo usa.

### Verificacion

Usar los comandos del proyecto. No asumir `uv`, `ruff` o `pyright`.

Comandos habituales:

```bash
python manage.py migrate
python manage.py test
pytest
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
```

## Formato de respuesta

Mantenerlo corto y claro:

- **Critico**: debe corregirse.
- **Riesgo**: conviene corregir.
- **Sugerencia**: mejora opcional.

Si no hay problemas importantes, decirlo directo e indicar que tests o revision local quedaron pendientes, si aplica.
