---
name: github-workflow
description: Simple GitHub workflow helper for issues, branches, commits, and PRs.
---

# GitHub Workflow

Usar este agente solo cuando el usuario pida ayuda con GitHub, issues, commits, ramas o PRs.

## Principios

- Mantener el flujo simple.
- Manejar ramas esta bien y puede ser el flujo normal de trabajo.
- Puedes crear una rama descriptiva para issues, bugs o cambios medianos.
- GitHub Actions o CI se pueden proponer si ayudan, pero no crearlas sin confirmacion.
- No hacer push, PR o merge sin que el usuario lo pida.
- Si hay un GitHub Issue, mantener el cambio alineado con ese alcance.

## Workflow de ramas

Flujo recomendado:

```bash
git status
git branch --show-current
git checkout -b descripcion-corta
```

Despues:

1. Implementar el cambio.
2. Correr migraciones si aplica.
3. Correr tests relevantes.
4. Levantar localhost.
5. Revisar `git status`.
6. Commit si el usuario lo pide o si forma parte del encargo.

## Nombres de ramas

Antes de cambios medianos o ligados a un issue:

```bash
git checkout -b descripcion-corta
```

Usar nombres simples:

```bash
fix-login
issue-12-ajustar-permisos
docker-dev-setup
```

No crear ramas nuevas para cambios pequenos si no aporta orden.

## GitHub Actions

No estan activadas por defecto en este template.

Si el usuario quiere empezar con Actions, proponer algo simple:

- correr tests en cada push a una rama;
- correr tests en pull requests;
- no hacer deploy automatico al principio.

## Issues

Si el usuario menciona un issue:

1. Leer el contexto disponible del issue.
2. Resumir brevemente objetivo, criterios y dudas.
3. Implementar solo lo necesario.
4. Reportar tests y revision local.

## Commits

Antes de commitear:

```bash
git status
git diff
```

Stagear solo archivos relevantes:

```bash
git add <archivos>
git commit -m "tipo: descripcion corta"
```

Usar mensajes simples. Conventional Commits esta bien, pero no forzarlo si el repo usa otro estilo.

## Pull Requests

Crear PR solo si el usuario lo pide.

La descripcion debe ser corta:

```markdown
## Resumen
- Cambio principal
- Tests ejecutados

## Revision local
- URL: http://localhost:8000/
```
