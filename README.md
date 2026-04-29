# Claude/Codex Django Template

Template de configuracion para usar Claude Code y Codex en proyectos Django con un flujo simple y revisable.

## Filosofia

- Django + Python.
- Preferencia por `pip` y `python manage.py`.
- Docker o Docker Compose segun la tarea.
- Migraciones antes de revisar en localhost.
- En proyectos nuevos, Django Admin habilitado para superusuario.
- Tests relevantes siempre; tests obligatorios en cambios criticos.
- Dialogos simples y directos.
- GitHub Issues cuando ayuden a ordenar trabajo.
- Workflow de ramas recomendado para ordenar cambios.
- GitHub Actions opcionales, no automaticas por defecto.

## Archivos principales

- [AGENTS.md](AGENTS.md): entrada principal para Codex.
- [CLAUDE.md](CLAUDE.md): memoria principal del proyecto.
- [.claude/settings.json](.claude/settings.json): hooks activos.
- [.claude/settings.md](.claude/settings.md): explicacion de hooks.
- [.claude/commands/fix.md](.claude/commands/fix.md): comando simple para arreglar cambios.
- [.claude/agents/code-reviewer.md](.claude/agents/code-reviewer.md): checklist de revision.
- [.claude/agents/github-workflow.md](.claude/agents/github-workflow.md): ayuda simple con issues, commits y PRs.
- [.claude/skills/README.md](.claude/skills/README.md): resumen de skills.
- [.mcp.json](.mcp.json): MCP opcional para GitHub Issues.
- [.github/workflows/README.md](.github/workflows/README.md): referencia opcional para Actions simples.

## Compatibilidad Claude y Codex

- Claude Code lee `CLAUDE.md` y usa `.claude/`.
- Codex lee `AGENTS.md`.
- `AGENTS.md` apunta a `CLAUDE.md` para no duplicar instrucciones.
- Codex puede leer `.claude/skills/` como documentacion, aunque no sean skills nativas de Codex.
- Los hooks de `.claude/hooks/` son para Claude Code; Codex no necesita ejecutarlos automaticamente.

## Flujo esperado

Despues de cambios relevantes en un proyecto Django:

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

Si el repo usa `pytest`:

```bash
pytest
docker compose exec web pytest
```

## Workflow de ramas

Para cambios medianos o ligados a issues:

```bash
git status
git checkout -b descripcion-corta
# cambios
python manage.py migrate
python manage.py test
git status
```

El push, merge y PR se hacen solo cuando el usuario lo pida.

## Hooks

Solo hay un hook activo:

- sugerir skills relevantes cuando se envia un prompt.

No hay hooks automaticos que corran lint, tipos, dependencias o tests despues de cada edicion. Claude debe revisar el proyecto y correr los comandos correctos explicitamente.

## Skills

Las skills principales son:

- `django-docker`: Docker, Compose, migraciones y localhost.
- `pytest-django-patterns`: tests con pytest o `manage.py test`.
- `systematic-debugging`: errores y bugs.
- `django-models`: modelos, migraciones y ORM.
- `django-forms`: formularios y validacion.
- `django-templates`: templates Django.
- `ticket`: trabajo desde GitHub Issues o tickets.

## Uso en otro proyecto

Copiar estos archivos al proyecto Django:

```bash
AGENTS.md
CLAUDE.md
.claude/
.mcp.json
.github/workflows/README.md
```

Luego ajustar nombres de servicios, comandos, base de datos y settings segun el proyecto real.
