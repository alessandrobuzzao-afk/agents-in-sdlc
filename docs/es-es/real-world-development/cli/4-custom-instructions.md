---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene proporcionar esa guía a Copilot. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizarás el archivo de instrucciones para garantizar que se sigan los estándares de programación;
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices y requisitos para el desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`; los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Con los archivos de instrucciones, proporcionarás a Copilot el contexto necesario para aplicar estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias, como convenciones de programación, bibliotecas y comentarios. Se pueden crear para todo el repositorio o para tipos concretos de archivos y tareas.

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general relevante para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md` proporcionan pautas para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> La disponibilidad de otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Examínalos antes de hacer cambios para observar el efecto:

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Lee la descripción breve del proyecto y las pautas de programación, aplicables a todas las interacciones con Copilot en este repositorio.
4. Explora `.github/instructions`, donde hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo`, que define el patrón de archivos al que se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y archivos del proyecto. Así se pueden dividir instrucciones amplias en archivos reutilizables más pequeños y señalar ejemplos.

## Actualiza las instrucciones para reflejar las pautas del equipo

Modifica `copilot-instructions.md` para solicitar comentarios TSDoc en el nuevo código TypeScript.

1. Busca la sección **Code formatting guidance**, aproximadamente en la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las nuevas pautas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar los cambios, sal de la conversación de filtrado y retómala para que Copilot aplique las pautas nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las pautas actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código correctamente.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI utiliza las instrucciones del repositorio y has aplicado un nuevo estándar a la funcionalidad. A continuación, [personalizarás y ejecutarás la habilidad reutilizable de comprobaciones de calidad][next-lesson] para ejecutar lint y pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
