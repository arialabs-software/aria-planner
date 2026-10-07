---
name: aria-planner
description: Activa el modo planificador de Aria. Sin argumentos solo confirma la activación; con argumentos planifica el objetivo indicado en docs/.
argument-hint: "[objetivo a planificar]"
---

# Modo aria-planner

Argumentos recibidos: `$ARGUMENTS`

## Qué hacer según los argumentos

- Si los argumentos están vacíos: responde únicamente "Modo AriaPlanner activado." y nada más. No leas archivos ni crees nada. Desde ese momento, aplica las instrucciones de abajo a cualquier objetivo que el usuario te plantee en esta sesión.
- Si hay argumentos: tómalos como el objetivo y trabájalo ya, aplicando las instrucciones de abajo.

## Instrucciones del modo

El agente debe:

* Analizar el objetivo general y el contexto del proyecto.
* Trabajar únicamente dentro de una nueva carpeta en `docs/` del proyecto correspondiente. En un monorepo, cada proyecto puede tener su propia documentación.
* Crear un `INDEX.md` con el contexto, objetivo, alcance y criterios generales del trabajo.
* Crear un `STATUS.md` para registrar qué partes del trabajo ya fueron completadas y cuál es el estado actual.
* Dividir el objetivo completo en artifacts pequeños, independientes y ejecutables.

Estructura recomendada:

```
docs/
└── [objective_name]/
    ├── INDEX.md
    ├── STATUS.md
    └── artifacts/
        ├── F01-...
        ├── F02-...
        ├── F03-...
        └── ...
```

Cada artifact debe representar una unidad de trabajo suficientemente pequeña como para que otro agente pueda ejecutarla, probarla y dejarla lista para auditoría.

## Límites

- No modifiques nada fuera de la nueva carpeta `docs/[objective_name]/`. Este modo planifica; no implementa.
