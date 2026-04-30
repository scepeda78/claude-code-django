# Claude/Codex Django Template

Template de configuracion para trabajar proyectos Django con Claude Code y Codex de forma ordenada, simple y revisable.

Incluye instrucciones compartidas, skills Django, hooks de sugerencia de skills, MCP opcional para GitHub y una skill UI/UX (`ui-ux-pro-max`) para diseno, accesibilidad, responsive y pulido visual.

## Para Que Sirve

Usa este template cuando quieras iniciar o estandarizar un proyecto Django con agentes AI.

El objetivo es que cada tarea siga un flujo claro:

1. Entender el requerimiento.
2. Revisar el repo antes de editar.
3. Implementar cambios pequenos y verificables.
4. Correr migraciones, tests y localhost cuando aplique.
5. Explicar lo hecho en lenguaje simple.

## Archivos Principales

| Archivo | Uso |
|---|---|
| [AGENTS.md](AGENTS.md) | Entrada principal para Codex |
| [CLAUDE.md](CLAUDE.md) | Memoria principal del proyecto para Claude Code |
| [.claude/](.claude/) | Settings, hooks, agentes y skills para Claude Code |
| [.codex/skills/](.codex/skills/) | Skills nativas para Codex |
| [.mcp.json](.mcp.json) | MCP opcional, por ejemplo GitHub |
| [.github/workflows/README.md](.github/workflows/README.md) | Referencia opcional para GitHub Actions |

## Instalacion En Un Proyecto Django

Desde este repo, copia los archivos base al proyecto destino:

```bash
rsync -a AGENTS.md CLAUDE.md .claude .codex .mcp.json .github /ruta/a/tu/proyecto/
```

Luego entra al proyecto destino:

```bash
cd /ruta/a/tu/proyecto
git status
```

Ajusta lo necesario:

- Nombre de servicios Docker.
- Base de datos y variables de entorno.
- Comandos de test si el proyecto usa `pytest`.
- Apps Django reales, settings, URLs y templates.
- `.mcp.json` si no usaras GitHub MCP.

Despues reinicia Claude Code o Codex para que lea las instrucciones nuevas.

## Instalacion MCP GitHub Opcional

El archivo [.mcp.json](.mcp.json) deja configurado GitHub MCP con `GITHUB_TOKEN`.

Ejemplo de variable de entorno:

```bash
export GITHUB_TOKEN="ghp_xxx"
```

Usalo si quieres trabajar con issues, PRs o contexto de GitHub desde el agente.

No guardes tokens reales en el repo.

## Flujo Base De Desarrollo

Para cambios normales:

```bash
git status
python manage.py migrate
python manage.py test
python manage.py runserver 0.0.0.0:8000
```

Si cambian modelos:

```bash
python manage.py makemigrations
python manage.py migrate
python manage.py test
python manage.py runserver 0.0.0.0:8000
```

Con Docker Compose:

```bash
docker compose build
docker compose up -d
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
docker compose logs --tail=100 web
```

El servidor local esperado es:

```text
http://localhost:8000/
```

## Como Ordenar Requerimientos

Antes de pedir una implementacion grande, conviene escribir el requerimiento en bloques. Esto evita cambios ambiguos y reduce retrabajo.

Usa este formato:

```markdown
# Requerimiento

## Objetivo
Que problema queremos resolver y para quien.

## Usuario
Quien usa esta funcionalidad: admin, cliente, equipo interno, visitante, etc.

## Alcance
Que entra en esta tarea.

## Fuera De Alcance
Que NO debe hacerse todavia.

## Datos
Modelos, campos, relaciones, permisos y reglas de negocio.

## Pantallas
Paginas, formularios, tablas, dashboards, emails o vistas necesarias.

## Flujo
Paso a paso de lo que hace el usuario.

## Estados
Loading, vacio, error, exito, sin permisos, datos incompletos.

## UI/UX
Estilo esperado, referencias, responsive, accesibilidad, colores o componentes.

## Validaciones
Reglas backend, errores esperados y casos borde.

## Tests
Flujos criticos que deben quedar cubiertos.
```

Ejemplo breve:

```markdown
# Requerimiento

## Objetivo
Permitir que un administrador cree clientes y vea su historial.

## Usuario
Administrador interno.

## Alcance
CRUD de clientes, listado con busqueda y detalle.

## Fuera De Alcance
Facturacion y reportes.

## Datos
Cliente: nombre, RUT, email, telefono, estado.

## Pantallas
Listado, formulario nuevo/editar, detalle.

## UI/UX
Tabla clara, filtros visibles, formulario con errores cerca del campo.

## Tests
Crear cliente valido, rechazar RUT duplicado, permisos de acceso.
```

## Secuencia Recomendada Para Un Proyecto

Para proyectos nuevos o medianos, avanzar en este orden:

1. **Onboarding del repo**
   - Revisar estructura, dependencias, settings, apps, URLs, modelos y tests.
   - Identificar si usa Docker, SQLite/Postgres, pytest, Celery, HTMX o frontend aparte.

2. **Base reproducible**
   - Crear o ajustar `.env.example`, `Dockerfile` y `docker-compose.yml` si hace falta.
   - Confirmar que `migrate`, `test` y `runserver` funcionen.

3. **Dominio y datos**
   - Definir modelos y relaciones.
   - Crear migraciones.
   - Agregar admin Django si es proyecto nuevo.

4. **Flujos backend**
   - Views, forms, permisos, servicios, validaciones y URLs.
   - Mantener validacion real en backend.

5. **UI inicial**
   - Templates base, navegacion, formularios, tablas y estados vacios.
   - Usar `django-templates` y `django-forms`.

6. **UI/UX y pulido visual**
   - Usar `ui-ux-pro-max` para dashboard, landing, responsive, accesibilidad, colores, tipografia, iconos o charts.
   - No agregar Tailwind/React/Vue si el repo no lo usa o el usuario no lo pidio.

7. **Tests y calidad**
   - Tests especificos del cambio.
   - Suite completa si toca permisos, modelos, pagos, datos sensibles o flujos criticos.

8. **Revision local**
   - Migraciones aplicadas.
   - Tests ejecutados.
   - Localhost levantado y URL indicada.

9. **Git**
   - Revisar `git status`.
   - Stagear solo archivos relevantes.
   - Commit y PR solo si el usuario lo pide.

## Cuando Usar UI/UX Pro Max

Usa `ui-ux-pro-max` cuando la tarea cambie como se ve o se usa una interfaz:

- Landing pages.
- Dashboards.
- Admin panels.
- Formularios.
- Tablas.
- Cards, botones, modales, navbars o sidebars.
- Charts o visualizacion de datos.
- Responsive mobile/desktop.
- Accesibilidad, foco, contraste, keyboard navigation.
- Colores, tipografia, iconos, spacing, sombras o animaciones.
- Pulido visual cuando algo "se ve poco profesional".

No hace falta usarlo para:

- Logica backend pura.
- Migraciones sin impacto visual.
- APIs sin interfaz.
- Scripts internos.
- Configuracion de infraestructura.

## Comandos UI/UX Pro Max

Codex:

```bash
python3 .codex/skills/ui-ux-pro-max/scripts/search.py "saas dashboard professional" --design-system -f markdown -p "Mi Proyecto"
python3 .codex/skills/ui-ux-pro-max/scripts/search.py "responsive forms" --stack html-tailwind
python3 .codex/skills/ui-ux-pro-max/scripts/search.py "data table accessibility" --domain ux
python3 .codex/skills/ui-ux-pro-max/scripts/search.py "trend comparison dashboard" --domain chart
```

Claude:

```bash
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "saas dashboard professional" --design-system -f markdown -p "Mi Proyecto"
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "responsive forms" --stack html-tailwind
```

Para guardar una guia visual del proyecto:

```bash
python3 .codex/skills/ui-ux-pro-max/scripts/search.py "clinic management dashboard clean" --design-system --persist -f markdown -p "Clinica"
```

Esto crea:

```text
design-system/clinica/MASTER.md
```

## Buenas Practicas Django

- Leer el repo antes de editar.
- Seguir patrones existentes.
- No introducir librerias nuevas si no hacen falta.
- Usar variables de entorno para secretos.
- No commitear `.env`.
- Usar `Form` o `ModelForm` para validar datos de usuario.
- Usar `{% url %}` en templates, no rutas hardcodeadas.
- Usar `{% static %}` para assets.
- Usar `select_related()` y `prefetch_related()` cuando haya riesgo de N+1.
- Crear migraciones solo cuando cambian modelos.
- No editar migraciones antiguas sin entender la historia.
- Mantener Django Admin habilitado en proyectos nuevos.

## Buenas Practicas UI/UX

- Mobile first: revisar al menos 375px, 768px, 1024px y 1440px.
- Contraste WCAG AA para texto normal.
- Focus visible en links, botones e inputs.
- Botones y acciones tactiles de al menos 44x44px.
- Formularios con labels visibles, errores cerca del campo y mensajes claros.
- Estados vacios utiles, no pantallas en blanco.
- Loading states para acciones lentas.
- No depender solo del color para comunicar estado.
- Usar iconos SVG o libreria existente, no emojis como iconos funcionales.
- Respetar `prefers-reduced-motion`.
- Reservar espacio para imagenes y contenido async para evitar saltos visuales.

## Ejemplos De Prompts Para Codex O Claude

Onboarding:

```text
Revisa este repo Django y resume estructura, dependencias, apps, settings, URLs, modelos, tests y como levantar localhost. No edites todavia.
```

Implementar una funcionalidad:

```text
Implementa el CRUD de clientes segun este requerimiento. Primero revisa los patrones actuales del repo, luego haz cambios pequenos, agrega tests relevantes, corre migraciones si hacen falta y deja localhost listo.
```

Trabajar con UI/UX:

```text
Mejora el dashboard para que sea responsive y mas claro. Usa ui-ux-pro-max para definir criterios de layout, accesibilidad, tablas y estados vacios. No agregues Tailwind ni React si el proyecto no los usa.
```

Trabajar desde issue:

```text
Revisa el issue #12, confirma alcance, implementa solo lo necesario, agrega tests relevantes y resume los cambios antes de commit.
```

Revision:

```text
Haz una revision de este cambio como code review: prioriza bugs, regresiones, permisos, datos sensibles y tests faltantes.
```

## Definition Of Done

Antes de entregar una tarea:

- El requerimiento principal esta implementado o el bloqueo esta explicado.
- Migraciones aplicadas o no necesarias.
- Tests relevantes ejecutados.
- Localhost levantado o bloqueo explicado.
- Cambios importantes explicados en lenguaje simple.
- `git status` revisado.
- No se tocaron archivos ajenos sin necesidad.

## Skills Incluidas

| Skill | Uso |
|---|---|
| `django-docker` | Docker, Compose, migraciones y localhost |
| `pytest-django-patterns` | Tests con pytest o `manage.py test` |
| `systematic-debugging` | Errores, bugs y tests fallando |
| `django-models` | Modelos, migraciones, ORM y consultas |
| `django-forms` | Formularios y validacion backend |
| `django-templates` | Templates, parciales y HTML Django |
| `ui-ux-pro-max` | UI/UX, responsive, accesibilidad, colores, tipografia, dashboards y landing pages |
| `ticket` | Trabajo desde GitHub Issues o tickets |

## Notas Para Agentes

- Instrucciones directas del usuario tienen prioridad.
- `AGENTS.md` manda para Codex.
- `CLAUDE.md` es memoria compartida del proyecto.
- `.claude/skills/` sirve como documentacion de apoyo.
- `.codex/skills/` contiene skills nativas de Codex.
- No hacer push, merge ni PR sin solicitud explicita.
