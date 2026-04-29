# Claude Code Settings

Configuracion pensada para proyectos Django con un flujo simple.

## Entorno

- `INSIDE_CLAUDE_CODE`: indica que los comandos corren dentro de Claude Code.
- `BASH_DEFAULT_TIMEOUT_MS`: 7 minutos para comandos normales.
- `BASH_MAX_TIMEOUT_MS`: 7 minutos como maximo.

## Hooks activos

### UserPromptSubmit

- Ejecuta `.claude/hooks/skill-eval.sh`.
- Sugiere skills relevantes segun el texto del pedido.
- No bloquea el trabajo.
- No obliga a usar una skill si no aplica.

## Hooks que no usamos por defecto

No hay hooks automaticos para formatear, instalar dependencias, correr `ruff`, correr `pyright` o ejecutar tests despues de cada edicion.

Motivo:

- No todos los proyectos usan las mismas herramientas.
- El flujo preferido es que Claude revise el repo y corra comandos explicitos.
- Para Django, las verificaciones importantes son migraciones, tests relevantes y localhost funcionando.

## Verificacion esperada por tarea

Claude debe elegir los comandos segun el proyecto:

```bash
python manage.py makemigrations   # solo si cambiaron modelos
python manage.py migrate
python manage.py test             # o pytest si el repo lo usa
python manage.py runserver 0.0.0.0:8000
```

Con Docker Compose:

```bash
docker compose build
docker compose up -d
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
```

Si el proyecto tiene `pytest`, `ruff` u otras herramientas configuradas, Claude puede usarlas, pero no debe asumir que existen.
