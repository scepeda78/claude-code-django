---
name: pytest-django-patterns
description: pytest and Django testing patterns. Use when writing tests, covering critical flows, fixing bugs, or choosing between pytest and python manage.py test.
---

# Django Tests

`pytest` esta bien si el proyecto ya lo usa o si conviene para organizar mejor los tests. Si no hay pytest configurado, usar `python manage.py test`.

## Regla principal

Cambios criticos siempre necesitan tests.

Critico incluye:

- login, permisos y datos sensibles;
- pagos, calculos de negocio y reportes;
- modelos importantes y migraciones;
- integraciones externas;
- bugs que ya afectaron comportamiento real.

## Flujo practico

1. Revisar como testea el repo actual.
2. Agregar el test mas chico que cubra el riesgo.
3. Correr el test especifico.
4. Correr una suite mas amplia si el cambio toca modelos, permisos, settings o flujos compartidos.

## Comandos

Con Django test runner:

```bash
python manage.py test
python manage.py test nombre_app
python manage.py test nombre_app.tests.TestEspecifico
```

Con pytest:

```bash
pytest
pytest nombre_app/tests/
pytest nombre_app/tests/test_archivo.py
pytest -k "nombre_del_test"
```

Con Docker Compose:

```bash
docker compose exec web python manage.py test
docker compose exec web pytest
```

## Que testear

- Vistas: status, redirects, permisos, contexto y efectos en base de datos.
- Formularios: datos validos, errores, `clean()` y guardado.
- Modelos: metodos propios, managers, constraints y migraciones riesgosas.
- Integraciones: mockear servicios externos, no llamar APIs reales.

## Criterio

No crear una infraestructura grande para cambios pequenos. Cubrir el riesgo real con tests claros.
