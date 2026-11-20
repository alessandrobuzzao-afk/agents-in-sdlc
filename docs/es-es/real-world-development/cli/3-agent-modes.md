---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Acuerda un enfoque con Plan, crea el filtrado a partir de una incidencia con Autopilot y revisa el resultado en el modo Interactive."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Los cambios mayores requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudarnos a crear lo adecuado de la manera adecuada. En esta lección iniciarás un flujo de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, validarás el código y el comportamiento y, finalmente, la combinarás en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama mientras continúas el flujo de trabajo de la funcionalidad. Normalmente se usarían ramas o solicitudes distintas para los diferentes tipos de archivo, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- validarás la funcionalidad manualmente en un navegador reenviado.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la solicitud de incorporación de cambios.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para decidir esos detalles y, después, autorizarás una implementación acotada con Autopilot.

## Información general

La IA no cambia los fundamentos del desarrollo. El flujo habitual consiste en partir de una incidencia, planificar el trabajo, crear y revisar el código, ejecutar pruebas, validar manualmente la funcionalidad, abrir una solicitud de incorporación de cambios y combinar el código una vez aprobado y superada la integración continua.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante de este enfoque.

Seguir este proceso ayuda a comprobar que el código generado por IA cumple los requisitos y pasa por las mismas revisiones que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar entre modos:

- **Interactive**: trabajas con el agente; propone cambios y espera tu respuesta antes de continuar.
- **Plan**: primero crea un plan y no puede editar archivos del proyecto.
- **Autopilot**: trabaja de forma autónoma, escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la solicitud de valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las flechas para seleccionar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

El prompt ahora empieza con `#7` (o un número similar). `#` permite incluir en el contexto una incidencia o solicitud de GitHub.

## Planifica el filtrado

Planificar permite acordar cómo implementar una funcionalidad antes de encargar el trabajo a Copilot.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan y confirma que el indicador muestra **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque.

3. Responde a las preguntas que haga sobre el filtrado según tus preferencias.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Revisa el plan y pide que modifique cualquier parte que no refleje tus decisiones.

## Aprueba Autopilot

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador muestra **Autopilot**.
3. Observa cómo Copilot implementa el plan y ejecuta las pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente; revísalo antes. Si Copilot informa de dependencias ausentes o conflictos de puerto, resuelve el problema antes de considerar completas las comprobaciones.

## Revisa y verifica la implementación

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones de planificación.
4. Pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones y pide a Copilot que corrija los errores.

## Explora la nueva funcionalidad

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando el puerto `4321` esté disponible en Codespaces, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones acordadas.
4. Comprueba que el restablecimiento y los resultados vacíos coincidan con la incidencia y el plan.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor.

## Resumen y próximos pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad: iniciaste una conversación desde una incidencia, definiste los requisitos en Plan, implementaste en Autopilot, revisaste el código y validaste manualmente el resultado.

A continuación, profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
