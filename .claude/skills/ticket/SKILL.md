---
name: ticket
description: Work from a GitHub Issue or ticket while keeping scope small. Use when the user provides an issue number, issue URL, or ticket context.
---

# Ticket / GitHub Issue

Usar cuando el usuario pida trabajar desde un issue o ticket.

## Pasos

1. Leer el contexto disponible del issue.
2. Resumir objetivo, criterios y dudas en pocas lineas.
3. Crear una rama descriptiva si el cambio lo amerita.
4. Revisar el codigo relacionado.
5. Implementar solo el alcance del issue.
6. Agregar tests si el cambio es critico o cambia comportamiento.
7. Correr migraciones si cambiaron modelos.
8. Correr tests relevantes.
9. Levantar localhost para revision.

## GitHub

- El usuario a veces usa GitHub Issues.
- GitHub Actions se pueden proponer si ayudan, pero no crearlas sin confirmacion.
- No crear PR, comentar issue o cambiar estados sin que el usuario lo pida.

## Entrega

Informar:

- archivos cambiados;
- tests ejecutados;
- migraciones aplicadas o no necesarias;
- URL local para revisar.
