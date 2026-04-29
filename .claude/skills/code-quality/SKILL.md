---
name: code-quality
description: Run configured project checks and summarize real issues. Use when the user asks to review code quality, run lint/tests, or verify a change before delivery.
---

# Code Quality

Revisar calidad sin asumir herramientas.

## Pasos

1. Leer `git diff` y detectar archivos relevantes.
2. Revisar si existen herramientas configuradas:
   - `pytest.ini`, `pyproject.toml` o `setup.cfg` para pytest.
   - configuracion de `ruff`, `black`, `mypy` o `pyright`.
   - `Dockerfile` o `docker-compose.yml`.
3. Correr checks disponibles.
4. Reportar solo problemas reales.

## Comandos comunes

Django:

```bash
python manage.py test
python manage.py migrate
```

pytest:

```bash
pytest
pytest ruta/de/tests/
```

Docker Compose:

```bash
docker compose exec web python manage.py test
docker compose exec web pytest
```

Lint o tipos, solo si estan configurados:

```bash
ruff check .
ruff format --check .
mypy .
pyright
```

## Checklist

- Tests relevantes pasan.
- Cambios criticos tienen cobertura.
- Migraciones creadas y aplicadas si cambiaron modelos.
- No hay secretos hardcodeados.
- No hay errores silenciosos importantes.
- No hay consultas N+1 evidentes.

## Respuesta

Mantenerlo corto:

- Que se corrio.
- Que fallo, si fallo algo.
- Que queda pendiente.
