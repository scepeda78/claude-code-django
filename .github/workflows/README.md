# GitHub Actions opcionales

Este template no activa Actions por defecto.

Si mas adelante quieres usar Actions, partir simple:

- correr tests en ramas;
- correr tests en pull requests;
- no hacer deploy automatico al principio.

Ejemplo minimo para un proyecto Django:

```yaml
name: Django tests

on:
  push:
    branches-ignore:
      - main
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install -r requirements.txt
      - run: python manage.py migrate
      - run: python manage.py test
```

Antes de crear una Action real, ajustar base de datos, variables de entorno y comandos del proyecto.
