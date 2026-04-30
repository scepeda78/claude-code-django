# Claude Code Skills

Skills para proyectos Django con un flujo simple: Docker cuando aplique, migraciones antes de localhost y tests relevantes, especialmente en cambios criticos.

## Skills principales

| Skill | Uso |
|-------|-----|
| [django-docker](./django-docker/SKILL.md) | Docker, Docker Compose, migraciones y localhost |
| [pytest-django-patterns](./pytest-django-patterns/SKILL.md) | Tests con pytest o `manage.py test` |
| [systematic-debugging](./systematic-debugging/SKILL.md) | Bugs, errores y tests fallando |
| [django-models](./django-models/SKILL.md) | Modelos, migraciones, ORM y consultas |
| [django-forms](./django-forms/SKILL.md) | Formularios y validacion backend |
| [django-templates](./django-templates/SKILL.md) | Templates, parciales y HTML Django |
| [ui-ux-pro-max](./ui-ux-pro-max/SKILL.md) | UI/UX, sistemas visuales, responsive, accesibilidad y pulido visual |
| [htmx-patterns](./htmx-patterns/SKILL.md) | Solo si el proyecto usa HTMX |
| [celery-patterns](./celery-patterns/SKILL.md) | Tareas en background |

## Workflow

| Skill | Uso |
|-------|-----|
| [onboard](./onboard/SKILL.md) | Entender un proyecto o tarea nueva |
| [ticket](./ticket/SKILL.md) | Trabajar desde un GitHub Issue o ticket |
| [code-quality](./code-quality/SKILL.md) | Correr checks configurados y revisar riesgos |
| [pr-review](./pr-review/SKILL.md) | Revisar un PR |
| [pr-summary](./pr-summary/SKILL.md) | Preparar resumen de cambios |
| [worktree-commit-merge](./worktree-commit-merge/SKILL.md) | Commit y merge si el usuario lo pide |
| [skill-creator](./skill-creator/SKILL.md) | Crear o actualizar skills |

## Reglas

- Las skills sugieren contexto, no reemplazan leer el repo.
- No asumir `uv`, `ruff`, `pyright`, Celery, HTMX o GitHub Actions si el proyecto no los usa.
- El workflow de ramas esta bien y puede usarse como flujo normal para cambios medianos.
- GitHub Actions se pueden proponer como mejora, pero no crearlas sin confirmacion.
- Para cambios criticos, agregar o actualizar tests.
- Antes de entregar para revision local: migrar, correr tests relevantes y levantar localhost.
- Mantener la comunicacion simple y directa.
