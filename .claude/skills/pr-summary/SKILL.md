---
name: pr-summary
description: Generate a short pull request summary for current branch changes. Use when the user asks for a PR description or change summary.
---

# PR Summary

Preparar un resumen corto de cambios.

## Pasos

```bash
git status
git diff --stat
git diff
```

Si existe rama principal:

```bash
git diff main...HEAD --stat
```

## Formato

```markdown
## Resumen
- Cambio principal
- Segundo cambio si aplica

## Tests
- Comando ejecutado y resultado

## Revision local
- URL local si quedo levantada
```
