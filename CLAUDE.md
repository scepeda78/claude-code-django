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
- **DOCKER-FIRST**: Todos los comandos Django, migraciones y tests se corren DENTRO de Docker, NUNCA localmente.
- `pytest` esta bien si el proyecto ya lo usa o si conviene para organizar mejor los tests.
- Mantener o crear archivos Docker para que el proyecto pueda levantarse de forma reproducible.
- Revisar cambios en `localhost` despues de modificar la app.
- Correr migraciones antes de levantar el servicio para revision local.
- En proyectos nuevos, dejar habilitado el Django Admin para el superusuario.
- Hacer y correr tests relevantes siempre que haya cambios de comportamiento.
- Para cambios criticos, siempre debe haber tests que cubran el flujo o riesgo principal.
- El usuario usa GitHub Issues a veces para organizar cambios.
- El manejo de ramas esta bien; crear una rama de trabajo cuando ayude a mantener ordenado el cambio.
- El workflow de ramas es util: rama, cambios, tests en Docker, localhost, commit y PR si el usuario lo pide.
- GitHub Actions no se crean por defecto; si pueden ayudar, explicarlo primero y pedir confirmacion.

## Reglas de trabajo

1. Antes de editar, revisa la estructura del proyecto, dependencias, settings, apps, URLs, modelos y tests existentes.
2. Sigue los patrones actuales del repo. No introduzcas librerias o herramientas nuevas si no son necesarias.
3. Si falta Docker, crea un `Dockerfile` y, cuando corresponda, un `docker-compose.yml`.
4. **TODOS** los comandos Python/Django/pytest se corren con `docker compose exec web ...`, NUNCA `python manage.py` a secas.
5. Si cambian modelos, crea migraciones con `docker compose exec web python manage.py makemigrations`.
6. Antes de que el usuario revise en localhost, corre `docker compose exec web python manage.py migrate`.
7. Levanta el servicio local despues de los cambios con Docker y entrega la URL, normalmente `http://localhost:8000/`.
8. Corre tests relevantes dentro de Docker. Si existe una suite clara, usa esa. Si no, usa `docker compose exec web python manage.py test`.
9. Si algo falla por configuracion, explica el bloqueo, el comando que fallo (con Docker) y el siguiente paso recomendado.
10. No borres ni reviertas cambios del usuario sin permiso.
11. No uses comandos destructivos como `git reset --hard` o borrados masivos sin confirmacion.
12. **Cambios quirurgicos**: cada linea que cambies debe trazar directo al pedido del usuario. No "mejores" codigo, comentarios ni formato de al lado. No refactorices lo que no esta roto. Si ves codigo muerto no relacionado, mencionalo; no lo borres. Si tu cambio deja imports o variables sin uso, esos si los limpias.
13. **Objetivo verificable antes de codear**: convierte la tarea en algo que se pueda comprobar. "Arregla el bug" → "escribe un test que lo reproduce, luego hazlo pasar". "Agrega validacion" → "tests con datos invalidos, luego hazlos pasar". Para tareas de varios pasos, di el plan corto con su verificacion por paso.

## Git y ramas

- Puedes revisar, crear y cambiar ramas cuando sea util para ordenar el trabajo.
- Para issues o cambios medianos, usar una rama descriptiva ayuda.
- **NUNCA commitear automaticamente. Siempre pedir aprobacion explicita del usuario antes de `git commit`.**
- No hacer push, merge o pull request sin que el usuario lo pida.
- Antes de commitear, revisar `git status` y stagear solo archivos relevantes. Mostrar al usuario que se va a commitear y esperar aprobacion.

Workflow recomendado (SIEMPRE CON DOCKER):

```bash
git status
git branch --show-current
git checkout -b descripcion-corta
# implementar cambios
docker compose build
docker compose up -d
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
docker compose logs --tail=50 web
# revisar en http://localhost:8000/
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

Despues de modificar codigo, **SIEMPRE** sigue este orden CON DOCKER:

```bash
docker compose build
docker compose up -d
docker compose exec web python manage.py makemigrations   # solo si cambiaron modelos
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
docker compose logs --tail=100 web
# revisar en http://localhost:8000/
```

No ejecutes comandos fuera de Docker. Todo debe correr dentro del contenedor.

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

### Comentarios en templates: `{# #}` es de UNA sola linea

El comentario corto de Django no es multilinea. Si abres `{#` y cierras `#}` en la linea siguiente, Django **no lo reconoce como comentario y escribe el texto tal cual en la pantalla**. En `sistemae2biz` este error aparecio 11 veces; una de ellas imprimio nombres de permisos que ese usuario no debia ver.

```django
{# bien: una sola linea #}

{% comment %}
  bien: varias lineas
{% endcomment %}

{# MAL: esto se imprime en la pagina
   porque el cierre quedo abajo #}
```

Antes de dar por terminado un cambio de templates, corre esta guardia:

```bash
docker compose exec web sh -c "grep -rn --include='*.html' '{#' templates/ */templates/ | grep -v '#}'"
```

Sin salida = todo bien. Con salida = esos comentarios se estan imprimiendo en pantalla.

## UI/UX

- `ui-ux-pro-max` esta instalado para Codex en `.codex/skills/ui-ux-pro-max/` y para Claude en `.claude/skills/ui-ux-pro-max/`.
- Usarlo cuando una tarea cambie como se ve o se usa una interfaz Django: templates, formularios, tablas, dashboards, landing pages, navegacion, estados visuales, responsive, accesibilidad, colores, tipografia o iconos.
- Preferir HTML/templates Django y CSS/static files existentes. No agregar React, Vue, Tailwind, shadcn/ui u otra cadena frontend si el proyecto no la usa o el usuario no lo pide.
- Si hace falta generar una guia visual, correr el buscador de la skill con `--design-system` y usar `--stack html-tailwind` como referencia cercana para templates Django.

## Diseño nuevo: se implementa igual, no se adapta

**Cuando el usuario entrega un diseño nuevo —imagenes, mockups, capturas, templates o una especificacion de front— el resultado debe quedar IGUAL a lo entregado.** Esta regla manda sobre la regla 2 («sigue los patrones actuales del repo»): si lo existente no calza, se cambia lo existente.

- **No adaptes el diseño a lo ya construido.** Adapta el codigo al diseño. Si una pantalla actual no se parece, se reescribe.
- **Crea lo que falte**: campos, migraciones, vistas, endpoints, JS, CSS, datos de prueba. «No existe el modelo» no es motivo para entregar algo parecido; es la lista de lo que hay que construir.
- **El copy va literal.** Cada titulo, bajada, estado vacio, chip y tooltip se copia palabra por palabra, con sus tildes. No lo mejores ni lo resumas.
- **Los detalles visuales son el encargo**, no adorno: colores, tonos de chip, `grid-template-columns`, paddings, tamaños de fuente, bordes.
- **Si un mockup contradice al repo, gana el mockup** — salvo que romperlo sea fallo de seguridad, de permisos o de aislamiento de datos. Ahi se avisa, se explica el choque y se espera decision.
- **Si una funcionalidad existente estorba al diseño nuevo**, dilo y pregunta antes de borrarla (regla 10 sigue vigente).
- **Los tests que afirmaban el diseño viejo se actualizan**, no frenan el cambio. Los que protegen comportamiento —permisos, alcance, aislamiento de datos— no se tocan: si esos fallan, el codigo nuevo esta mal.

### Comparar contra la imagen, no contra el codigo

Antes de dar por terminado, **compara la pantalla real renderizada contra la imagen entregada y enumera lo que quedo distinto**. Si algo no se pudo replicar, dilo con el motivo.

Comparar a ojo, o comparar contra los `.html` del paquete en vez de contra las imagenes, entrega pantallas *parecidas* en vez de iguales. Tampoco detecta un contenedor sirviendo codigo viejo. Si el proyecto tiene Selenium o Playwright, saca capturas de verdad a 1440px de ancho y mira el resultado.

Dos cosas que solo se ven mirando **todas las pantallas juntas** en una sesion, nunca leyendo el diff de una rama sola:

- Tablas con `min-width` que empujan el boton principal fuera del borde visible con el menu abierto. Un boton que hay que buscar es un boton que no se aprieta.
- Avisos o pasos siguientes que desaparecen al cambiar de estado, dejando al usuario sin el camino que la pantalla acababa de prometer.

## CSS y Estilos

**REGLA FUNDAMENTAL: Todos los estilos reutilizables viven en `base.html` como clases CSS semánticas. NO inline styles excepto raras excepciones.**

- Define estilos reutilizables (botones, formularios, cards, colores, espaciado, tipografia) en el `<style>` de `base.html`.
- Usa nombres de clase semánticos: `.btn`, `.btn-primary`, `.form-field`, `.card`, `.section-title`, etc.
- Las templates heredan y usan esas clases con `class="..."`.
- Inline `style="..."` **solo** en casos excepcionales: valores computados desde BD, one-off unico, o estilos dinamicos que varían por dato.
- Antes de agregar estilos nuevos, revisa si ya existen clases reutilizables en `base.html`.

Ejemplo correcto:

```html
<!-- base.html -->
<style>
  .btn {
    background: #007bff;
    padding: 10px 15px;
    border-radius: 5px;
    color: white;
    font-weight: 600;
  }
  .btn-primary { background: #007bff; }
  .btn-danger { background: #dc3545; }
</style>

<!-- template.html -->
<button class="btn btn-primary">Enviar</button>
<button class="btn btn-danger">Eliminar</button>
```

No hagas esto:

```html
<!-- ❌ INCORRECTO -->
<button style="background: #007bff; padding: 10px 15px; border-radius: 5px; color: white;">Enviar</button>
```

### Tokens semanticos, no hex sueltos

Cuando el proyecto crezca de un puñado de clases, define primero variables CSS con nombre **semantico** (que significa) y no literal (que color es). Asi un estado cambia en un solo lugar y ningun template repite un hex.

```css
:root {
  --ok-bg:    #DCFCE7;  --ok-fg:    #15803D;
  --warn-bg:  #FEF9C3;  --warn-fg:  #854D0E;
  --alert-bg: #FEE2E2;  --alert-fg: #991B1B;
  --info-bg:  #E0F2FE;  --info-fg:  #075985;
  --mute-bg:  #F1F5F9;  --mute-fg:  #475569;
  --linea: #E2E8F0;
  --radio: 12px;
}
.chip-ok { background: var(--ok-bg); color: var(--ok-fg); border-radius: var(--radio); }
```

Reglas que se ganaron en la practica:

- Un color decorativo **no puede leerse como un estado**. Si asignas colores a personas (avatares, etiquetas), usalos frios y distintos de la paleta de ok/warn/alert.
- Un color derivado de un dato se deriva del **id**, no del nombre: si se deriva del nombre, cambia al renombrar a alguien.
- Todo par fondo/texto debe pasar contraste AA (4.5:1). Verificalo antes de agregar un tono.
- Elementos que deben seguir funcionando aunque la pantalla este en solo-lectura (cerrar sesion, menu de cuenta) van con `<details>/<summary>`, no con `<button>`: un barrido que deshabilita botones no apaga un `<summary>`.

## Tests

Los tests son obligatorios para cambios criticos: permisos, pagos, login, datos sensibles, modelos importantes, migraciones, calculos de negocio, reportes, integraciones externas y cualquier flujo que pueda romper trabajo real del usuario.

Prioridad:

1. Tests especificos del cambio.
2. Tests de la app modificada.
3. Suite completa si el cambio toca modelos, settings, middleware, permisos o flujos criticos.

**TODOS los tests se corren DENTRO de Docker:**

```bash
docker compose exec web python manage.py test
docker compose exec web python manage.py test nombre_app
docker compose exec web python manage.py test nombre_app.tests.TestEspecifico
```

Si el repo usa `pytest`:

```bash
docker compose exec web pytest
docker compose exec web pytest nombre_app/tests/
docker compose exec web pytest nombre_app/tests/test_archivo.py
```

Para crear superusuario en proyectos nuevos:

```bash
docker compose exec web python manage.py createsuperuser
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
