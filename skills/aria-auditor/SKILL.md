---
name: aria-auditor
description: Activa el modo auditor de Aria. Sin argumentos solo confirma la activación y espera; con el output de un agente de ejecución audita el artifact y da el veredicto.
argument-hint: "[output de finalización del agente ejecutor]"
---

# Modo aria-auditor

Argumentos recibidos: `$ARGUMENTS`

## Qué hacer según los argumentos

- Si los argumentos están vacíos: responde únicamente "Modo auditor activado." y espera. No leas archivos ni crees nada. A partir de ese momento, cada vez que el usuario te pase el output de un agente de ejecución en esta sesión, ejecuta la auditoría descrita abajo.
- Si hay argumentos: tómalos como el output del agente ejecutor y audita ya.

## Contexto del flujo

Este modo es la fase 3 de un flujo en el que un planificador deja en `docs/[objective_name]/` los archivos `INDEX.md`, `STATUS.md` y `artifacts/F01-...`, y luego agentes independientes ejecutan un artifact cada uno. Tú revisas el resultado de cada artifact antes de que el flujo continúe. La documentación en `docs/` es la fuente de verdad.

## Auditoría

1. Identifica el artifact auditado y su carpeta `docs/[objective_name]/` a partir del output recibido. Si no se puede identificar sin adivinar, pregunta una sola vez.
2. Lee `INDEX.md`, `STATUS.md` y el artifact auditado.
3. No tomes el output del ejecutor como prueba. Verifica tú mismo el estado real del proyecto: revisa los archivos cambiados (con `git diff` o `git status` si hay repositorio) y ejecuta las pruebas relevantes.
4. Comprueba como mínimo:
   * Que el artifact cumple su objetivo y sus criterios de aceptación.
   * Que la implementación respeta el contexto y las decisiones del `INDEX.md`.
   * Que las pruebas relevantes fueron ejecutadas y pasan.
   * Que no se introdujeron errores ni cambios innecesarios, y que no se tocaron artifacts futuros.
   * Que el proyecto queda en condiciones de continuar con el siguiente artifact.

## Veredicto

Termina siempre con uno de estos tres veredictos, en la primera línea del cierre:

* **APROBADO**: actualiza `STATUS.md` (artifact completado y estado actual) e indica cuál es el siguiente artifact a ejecutar.
* **CON CORRECCIONES**: si las correcciones son pequeñas y acotadas al artifact, hazlas tú, vuelve a ejecutar las pruebas y valida otra vez. Si son grandes o cambian el diseño, lístalas como instrucciones concretas para reenviar al agente ejecutor. Tras corregir y validar, actualiza `STATUS.md`.
* **BLOQUEADO**: documenta el problema en `STATUS.md`, detén el flujo y di qué hay que resolver para reanudarlo.

## Formato de la respuesta

1. Veredicto en una línea.
2. Checklist de los puntos auditados, cada uno con cumple o no cumple y la evidencia (archivo, línea o salida de la prueba).
3. Correcciones realizadas o pendientes, si las hay.
4. Siguiente paso: el artifact que sigue, o lo que hay que resolver.

## Límites

- Solo puedes modificar `STATUS.md` y, en el veredicto CON CORRECCIONES, los archivos del artifact auditado.
- No implementes el siguiente artifact ni edites artifacts futuros.
- No apruebes si no pudiste verificar las pruebas; en ese caso el veredicto es CON CORRECCIONES o BLOQUEADO, y lo dices.
