---
name: loop-triage
description: Daily triage loop for Django projects. Report-only health check that reads STATE.md, checks tests, migrations, git and issues, and appends prioritized findings without fixing anything. Use with /loop or on a schedule.
---

# Loop Triage

Triage diario del proyecto, solo reporte. Basado en el patron "Daily Triage" de loop-engineering (L1: observar, no arreglar).

## Setup (una vez)

1. Copiar `STATE.md.example` a `STATE.md` en la raiz del proyecto.
2. Lanzar el loop:

```
/loop 1d Corre $loop-triage. Lee STATE.md primero. Agrega items de alta prioridad y watch list. Actualiza "Last run". NO arregles nada automaticamente.
```

## Pasos por corrida (SIEMPRE con Docker)

1. Leer `STATE.md` — que sabe ya el loop.
2. Recolectar senales:
   - `docker compose exec web python manage.py test` (o `pytest` si el repo lo usa)
   - `docker compose exec web python manage.py makemigrations --check --dry-run` (migraciones pendientes)
   - `git log --oneline -15` y `git status` (cambios recientes, archivos sueltos)
   - `gh issue list --limit 15` si el repo usa GitHub Issues
   - `docker compose logs --tail=100 web` (errores en runtime)
3. Clasificar en el reporte.
4. Actualizar `STATE.md`: nuevos items, timestamp, run log.

## Formato del reporte

### 1. Alta prioridad (actuar hoy)
- Descripcion en una linea, por que importa, siguiente accion sugerida, esfuerzo estimado.

### 2. Watch (monitorear, no actuar)
- Mismo formato, menor urgencia.

### 3. Ruido (revisado, ignorado)
- Lista breve.

### 4. Actualizaciones de estado
- Hechos a recordar para la proxima corrida.

## Reglas

- Solo reporte. NUNCA auto-fix, NUNCA commit, NUNCA push.
- Brutal y conciso. En duda, va a Watch o Ruido, no a alta prioridad.
- No proponer refactors ni cambios de arquitectura — esto es senal, no invencion.
- Todo comando Python/Django dentro de Docker (`docker compose exec web ...`).
- Si Docker no esta levantado, reportarlo como item y no correr comandos locales.
- Para pasar a L2 (auto-fix de items chicos con worktree + verificacion), el usuario debe pedirlo explicitamente despues de 1-2 semanas de triage confiable.
