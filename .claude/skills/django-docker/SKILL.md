---
name: django-docker
description: Docker and Docker Compose workflow for Django projects. Use when setting up Docker, running migrations, starting localhost, or making the project reproducible locally.
---

# Django Docker

Usar esta skill cuando el proyecto necesite Docker, Docker Compose, migraciones o revision en localhost.

## Principios

- Si falta Docker, crear `Dockerfile`.
- Si hay base de datos, Redis, Celery u otro servicio, crear o ajustar `docker-compose.yml`.
- Usar `.env` o `.env.example` para configuracion. No hardcodear secretos.
- Antes de que el usuario revise localhost, correr migraciones.
- En proyectos nuevos, dejar Django Admin habilitado para el superusuario.
- Dejar el servicio levantado e indicar la URL local.

## Flujo con Docker Compose

```bash
docker compose build
docker compose up -d
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
docker compose logs --tail=100 web
```

Si el proyecto usa `pytest`:

```bash
docker compose exec web pytest
```

## Flujo sin Compose

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py test
python manage.py runserver 0.0.0.0:8000
```

## Checklist

- `Dockerfile` existe o fue actualizado.
- `docker-compose.yml` existe si hace falta.
- Migraciones aplicadas.
- Django Admin disponible en `/admin/` si es un proyecto nuevo.
- Tests relevantes ejecutados.
- Servidor levantado en `http://localhost:8000/` o explicado si no se pudo.
