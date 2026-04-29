Revisa los archivos modificados, identifica el problema y arreglalo con el menor cambio razonable.

Mantener el flujo simple:

1. Revisar `git diff` y los archivos tocados.
2. Si es Django, revisar modelos, vistas, URLs, templates y tests relacionados.
3. Si cambiaron modelos, correr `python manage.py makemigrations`.
4. Antes de revisar en localhost, correr `python manage.py migrate`.
5. Correr tests relevantes:
   - `pytest ...` si el proyecto usa pytest.
   - `python manage.py test ...` si no hay pytest.
6. Si existe Docker Compose, preferir:
   - `docker compose build`
   - `docker compose up -d`
   - `docker compose exec web python manage.py migrate`
   - `docker compose exec web python manage.py test`
7. Levantar el servicio y dejar la URL local lista para revisar.

Si hay herramientas como `ruff`, `black`, `pyright` o `mypy`, usarlas solo si el proyecto ya las tiene configuradas.

No ocultar errores con `# noqa`, `# type: ignore` o cambios grandes sin explicar por que son necesarios.
