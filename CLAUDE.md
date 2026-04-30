# CLAUDE.md - Template para proyectos Django

Este archivo define como debe trabajar Claude Code en mis proyectos Django. Usalo como memoria del proyecto y ajusta los comandos si el repo ya tiene una convencion distinta.

Para compatibilidad con Codex, mantener `AGENTS.md` en la raiz del proyecto. `AGENTS.md` debe apuntar a este archivo como fuente principal para no duplicar reglas.

## Perfil del usuario

- El usuario no es programador experto.
- Explica los cambios importantes en lenguaje claro y con ejemplos concretos cuando ayude.
- Mantiene los dialogos lo mas simples y directos posible.
- No hagas cambios grandes de arquitectura sin explicar antes el motivo.
- Evita respuestas excesivamente tecnicas si no son necesarias.
- Cuando termines, indica que archivos cambiaste, que comandos corriste y que queda listo para revisar en localhost.

## Preferencias generales

- Stack principal: Django + Python.
- Gestor preferido: `pip`.
- Comandos Django preferidos: `python manage.py ...`.
- `pytest` esta bien si el proyecto ya lo usa o si conviene para organizar mejor los tests.
- Usar Docker o Docker Compose segun la tarea.
- Mantener o crear archivos Docker para que el proyecto pueda levantarse de forma reproducible.
- Revisar cambios en `localhost` despues de modificar la app.
- Correr migraciones antes de levantar el servicio para revision local.
- En proyectos nuevos, dejar habilitado el Django Admin para el superusuario.
- Hacer y correr tests relevantes siempre que haya cambios de comportamiento.
- Para cambios criticos, siempre debe haber tests que cubran el flujo o riesgo principal.
- El usuario usa GitHub Issues a veces para organizar cambios.
- El manejo de ramas esta bien; crear una rama de trabajo cuando ayude a mantener ordenado el cambio.
- El workflow de ramas es util: rama, cambios, tests, localhost, commit y PR si el usuario lo pide.
- GitHub Actions no se crean por defecto; si pueden ayudar, explicarlo primero y pedir confirmacion.

## Reglas de trabajo

1. Antes de editar, revisa la estructura del proyecto, dependencias, settings, apps, URLs, modelos y tests existentes.
2. Sigue los patrones actuales del repo. No introduzcas librerias o herramientas nuevas si no son necesarias.
3. Si falta Docker, crea un `Dockerfile` y, cuando corresponda, un `docker-compose.yml`.
4. Si cambian modelos, crea migraciones con `python manage.py makemigrations`.
5. Antes de que el usuario revise en localhost, corre `python manage.py migrate`.
6. Levanta el servicio local despues de los cambios y entrega la URL, normalmente `http://localhost:8000/`.
7. Corre tests relevantes. Si existe una suite clara, usa esa. Si no, usa `python manage.py test`.
8. Si algo falla por configuracion local, explica el bloqueo, el comando que fallo y el siguiente paso recomendado.
9. No borres ni reviertas cambios del usuario sin permiso.
10. No uses comandos destructivos como `git reset --hard` o borrados masivos sin confirmacion.

## Git y ramas

- Puedes revisar, crear y cambiar ramas cuando sea util para ordenar el trabajo.
- Para issues o cambios medianos, usar una rama descriptiva ayuda.
- No hacer push, merge o pull request sin que el usuario lo pida.
- Antes de commitear, revisar `git status` y stagear solo archivos relevantes.

Workflow recomendado:

```bash
git status
git branch --show-current
git checkout -b descripcion-corta
# implementar cambios
python manage.py migrate
python manage.py test
python manage.py runserver 0.0.0.0:8000
git status
```

## Skills y agentes

- Usa skills y agentes como apoyo contextual, no como pasos obligatorios.
- Si el hook sugiere una skill, aplicala solo si realmente calza con la tarea.
- No actives automatizaciones complejas si una lectura directa del repo basta.
- Prioriza `django-docker`, `pytest-django-patterns` y `systematic-debugging` cuando correspondan.
- Para cambios de UI, templates, dashboards, landing pages, responsive, accesibilidad, colores, tipografia o pulido visual, usa `ui-ux-pro-max` como apoyo junto con `django-templates`, `django-forms` o `htmx-patterns` cuando aplique.

## Docker

Siempre intenta que el proyecto pueda ejecutarse con Docker. Usa:

- `Dockerfile` para construir la imagen de la app.
- `docker-compose.yml` cuando haya servicios como PostgreSQL, Redis, Celery, workers o cuando sea mas comodo levantar todo junto.
- Variables de entorno en `.env` o `.env.example`; nunca hardcodear secretos.

Comandos preferidos con Compose:

```bash
docker compose build
docker compose up -d
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
docker compose logs -f web
```

Si el proyecto no usa Compose todavia:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py test
python manage.py runserver 0.0.0.0:8000
```

### Dockerfile base

Usa este punto de partida cuando el proyecto no tenga Dockerfile. Ajusta el nombre del proyecto, dependencias del sistema y comandos segun el repo.

```dockerfile
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

RUN apt-get update \
    && apt-get install -y --no-install-recommends build-essential libpq-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements*.txt ./
RUN pip install --upgrade pip \
    && if [ -f requirements.txt ]; then pip install -r requirements.txt; fi

COPY . .

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

### docker-compose.yml base

Usa Compose para desarrollo local, especialmente si hay base de datos.

```yaml
services:
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    env_file:
      - .env
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_DB: django
      POSTGRES_USER: django
      POSTGRES_PASSWORD: django
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
```

Si el proyecto usa SQLite, puedes crear Compose solo con el servicio `web` y sin `db`.

## Flujo despues de cada cambio

Despues de modificar codigo, sigue este orden:

```bash
python manage.py makemigrations   # solo si cambiaron modelos
python manage.py migrate
python manage.py test
python manage.py runserver 0.0.0.0:8000
```

Si se usa Docker Compose:

```bash
docker compose build
docker compose up -d
docker compose exec web python manage.py makemigrations   # solo si cambiaron modelos
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
docker compose logs --tail=100 web
```

Antes de terminar una tarea, confirma:

- Migraciones aplicadas o no necesarias.
- Tests relevantes ejecutados.
- Servicio levantado para revisar en localhost.
- URL local indicada al usuario.

## Django

- Mantener settings separados para desarrollo/produccion si el repo ya lo hace.
- En proyectos nuevos, incluir `django.contrib.admin`, registrar la ruta `admin/` y asegurar que se pueda crear/acceder con superusuario.
- Usar variables de entorno para secretos, claves, hosts, base de datos y servicios externos.
- No commitear `.env` con secretos reales.
- Usar `select_related()` y `prefetch_related()` cuando haya riesgo de consultas N+1.
- Validar formularios con `Form` o `ModelForm`; no confiar solo en validaciones de frontend.
- Mantener templates simples y reutilizar parciales cuando el repo ya tenga ese patron.
- Si una vista cambia comportamiento visible, agregar o actualizar tests.
- Si una migracion falla, no editar migraciones antiguas sin entender la historia del proyecto.

## UI/UX

- `ui-ux-pro-max` esta instalado para Codex en `.codex/skills/ui-ux-pro-max/` y para Claude en `.claude/skills/ui-ux-pro-max/`.
- Usarlo cuando una tarea cambie como se ve o se usa una interfaz Django: templates, formularios, tablas, dashboards, landing pages, navegacion, estados visuales, responsive, accesibilidad, colores, tipografia o iconos.
- Preferir HTML/templates Django y CSS/static files existentes. No agregar React, Vue, Tailwind, shadcn/ui u otra cadena frontend si el proyecto no la usa o el usuario no lo pide.
- Si hace falta generar una guia visual, correr el buscador de la skill con `--design-system` y usar `--stack html-tailwind` como referencia cercana para templates Django.

## Tests

Los tests son obligatorios para cambios criticos: permisos, pagos, login, datos sensibles, modelos importantes, migraciones, calculos de negocio, reportes, integraciones externas y cualquier flujo que pueda romper trabajo real del usuario.

Prioridad:

1. Tests especificos del cambio.
2. Tests de la app modificada.
3. Suite completa si el cambio toca modelos, settings, middleware, permisos o flujos criticos.

Comandos comunes:

```bash
python manage.py test
python manage.py test nombre_app
python manage.py test nombre_app.tests.TestEspecifico
```

Para crear superusuario en proyectos nuevos:

```bash
python manage.py createsuperuser
```

Si el repo usa `pytest`, respeta esa configuracion:

```bash
pytest
pytest nombre_app/tests/
pytest nombre_app/tests/test_archivo.py
```

No inventes una infraestructura grande de tests para cambios pequenos. Agrega lo minimo necesario para cubrir el riesgo real, pero no dejes cambios criticos sin cobertura.

## GitHub Issues

- Si el usuario menciona un issue, revisa el contexto del issue antes de implementar cuando tengas acceso.
- Mantiene el alcance alineado con el issue.
- Puedes sugerir una lista corta de subtareas para registrar en GitHub Issues.
- No crees GitHub Actions o automatizaciones CI salvo solicitud explicita.

## Comunicacion esperada

Durante el trabajo:

- Explica brevemente que estas revisando o cambiando.
- Si necesitas instalar dependencias, migrar o levantar Docker, dilo antes.
- Si hay un error, muestra el comando relevante y la causa probable.
- Mantiene los mensajes simples: primero lo importante, despues el detalle solo si ayuda.

Al terminar:

- Resume cambios en 2 a 5 bullets.
- Indica comandos ejecutados y resultado.
- Indica si el servidor quedo levantado y en que URL.
- Menciona cualquier pendiente o decision que el usuario deba revisar.

## Definicion de listo

Una tarea esta lista cuando:

- El codigo esta implementado.
- Las migraciones fueron creadas si hacian falta.
- `migrate` fue ejecutado antes de levantar localhost.
- Los tests relevantes fueron ejecutados.
- El servicio esta disponible en localhost o se explico claramente por que no pudo levantarse.
- El usuario recibio una explicacion clara, sin asumir conocimiento avanzado.
