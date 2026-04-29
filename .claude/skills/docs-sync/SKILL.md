---
name: docs-sync
description: Check if documentation is in sync with Django code. Use when the user asks to verify docs, README files, or setup instructions.
---

# Docs Sync

Revisar que la documentacion no contradiga el codigo.

## Pasos

1. Revisar cambios recientes:
   ```bash
   git diff --stat
   git diff
   ```
2. Buscar README o docs relacionados.
3. Confirmar que comandos, rutas, variables de entorno y nombres de apps sigan siendo correctos.
4. Reportar solo problemas reales.

## Criterio

- No pedir documentacion extra por costumbre.
- No crear docs largas si basta con una nota corta.
- Si cambia Docker, setup, migraciones o tests, actualizar instrucciones de uso.
