# AGENTS.md

Instrucciones para Codex en proyectos Django.

## Fuente principal

Usar `CLAUDE.md` como la fuente principal de instrucciones del proyecto.

Si hay conflicto entre archivos:

1. Seguir primero instrucciones directas del usuario.
2. Seguir `AGENTS.md` para comportamiento de Codex.
3. Usar `CLAUDE.md` como memoria compartida del proyecto.
4. Usar `.claude/skills/` como documentacion de apoyo.
5. Usar `.codex/skills/` como skills nativas de Codex cuando existan.

## Flujo base

- Django + Python.
- Preferir `pip` y `python manage.py`.
- Usar Docker o Docker Compose cuando aplique.
- Crear/mantener Docker en proyectos nuevos o cuando sea necesario.
- Correr migraciones antes de levantar localhost.
- Levantar el servicio para revisar en `http://localhost:8000/`.
- Tests relevantes siempre; tests obligatorios en cambios criticos.
- `pytest` esta bien si el proyecto lo usa.
- En proyectos nuevos, habilitar Django Admin para el superusuario.
- Mantener dialogos simples y directos.
- Para cambios visuales, templates, dashboards, landing pages o UI/UX, usar `ui-ux-pro-max` junto con las skills Django que apliquen.

## Git

- El workflow de ramas esta permitido y recomendado para cambios medianos.
- Puedes crear una rama descriptiva cuando ayude.
- No hacer push, merge ni pull request sin que el usuario lo pida.
- Antes de commitear, revisar `git status` y stagear solo archivos relevantes.

## Claude compatibility

- `.claude/` es para Claude Code.
- `.codex/skills/` es para Codex.
- Codex puede leer `.claude/skills/` como documentacion, pero no debe asumir que son skills nativas de Codex.
- Los hooks de `.claude/hooks/` son de Claude Code; Codex no necesita ejecutarlos automaticamente.
- `.mcp.json` puede servir para acceso MCP, por ejemplo GitHub, si el entorno lo soporta.

## Checklist antes de entregar

- Migraciones aplicadas o no necesarias.
- Tests relevantes ejecutados.
- Localhost levantado o bloqueo explicado.
- Cambios importantes explicados en lenguaje simple.
