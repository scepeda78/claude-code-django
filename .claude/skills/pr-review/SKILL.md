---
name: pr-review
description: Review a pull request using the project standards. Use when the user asks to review a PR or compare branch changes.
---

# PR Review

Revisar un PR con foco en riesgos reales.

## Pasos

1. Obtener diff del PR o branch.
2. Leer `.claude/agents/code-reviewer.md`.
3. Revisar bugs, permisos, datos, migraciones y tests.
4. No exigir herramientas que el repo no usa.
5. No publicar comentarios en GitHub salvo que el usuario lo pida.

## Respuesta

Primero hallazgos:

- **Critico**: debe corregirse.
- **Riesgo**: conviene corregir.
- **Sugerencia**: opcional.

Despues, indicar tests o revision local pendiente.
