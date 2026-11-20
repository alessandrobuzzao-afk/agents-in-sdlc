---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información adicional, conviene proporcionarle ese contexto. Los [archivos de instrucciones][instruction-files] indican cómo generar el código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos específicos de rutas;
- generarás la primera parte de la capa de datos para el filtrado (un helper de editoriales) con las instrucciones actuales;
- añadirás un estándar general a `.github/copilot-instructions.md`;
- repetirás el prompt y observarás cómo el código generado sigue el nuevo estándar;
- confirmarás los cambios en `main` para que Copilot use las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot es probabilístico. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener siempre el mismo resultado.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias pautas para el desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Los archivos de instrucciones ayudarán a Copilot a respetar estas prácticas.

### Instrucciones personalizadas

Las instrucciones permiten dar a Copilot contexto y preferencias, como convenciones de programación, bibliotecas y comentarios. Se pueden crear para todo el repositorio o para tareas concretas.

- `.github/copilot-instructions.md` se envía a Copilot en **todas** las solicitudes del repositorio y debe contener información general del proyecto.
- `.github/instructions/*.instructions.md` contiene pautas para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat; no se aplican a las sugerencias de finalización o siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot admite también otros formatos, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad][custom-instructions-support].

### Prácticas recomendadas para gestionar instrucciones

- Mantén `copilot-instructions.md` centrado en el proyecto, su estructura y estándares globales.
- Usa `*.instructions.md` para lenguajes, tipos de archivo y tareas concretos.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos.

No hay una única manera correcta de utilizar la IA; experimenta y ajusta las instrucciones al proyecto.

> [!TIP]
> Copilot puede ayudarte a generar archivos de instrucciones. Según el entorno, puedes usar **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. Para obtener plantillas, consulta [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

1. Abre `.github/copilot-instructions.md` y revisa secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**, incluidas las pautas anidadas para GitHub Actions.
2. Explora `.github/instructions`, donde hay instrucciones para Astro, Drizzle, pruebas y otras tareas.
3. Abre `.github/instructions/unit-tests.instructions.md` y observa `applyTo`, que define el patrón aplicable, por ejemplo `**/*.test.ts`.
4. Revisa las pautas para crear pruebas unitarias.
5. Abre `.github/instructions/drizzle.instructions.md` y consulta sus enlaces a otras instrucciones y ejemplos del proyecto. Las rutas son relativas a ese archivo.

> [!NOTE]
> **Code formatting requirements** documenta los estándares, pero aún no exige documentación en el código. Añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crea una rama para los cambios

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> En el Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indica las opciones necesarias.

Primero enviarás un prompt con las instrucciones actuales. Después las actualizarás y repetirás el mismo prompt para comparar el resultado.

1. Cierra las pestañas de editor que no necesites.
2. Abre `src/lib/publishers.ts` para indicar dónde debe estar el helper.
3. En la vista Chat, selecciona **Agent** en el selector de agentes.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, normalmente al helper y sus pruebas.
6. Observa que el helper es una función tipada que recibe un cliente `db` y devuelve una matriz tipada de editoriales; esto procede de las convenciones de `.github/instructions/drizzle.instructions.md`, aplicables a `src/lib/*.ts`.
7. Observa que el código propuesto no incluye comentarios TSDoc ni un encabezado de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que genere comentarios aunque no se le hayan pedido. No pasa nada: el objetivo es observar si mejora la coherencia tras actualizar las instrucciones.

## Añade un estándar al repositorio

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements**, aproximadamente en la línea 27. Aún no incluye reglas para documentar el código.
3. Añade estas líneas después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del repositorio van en `.github/copilot-instructions.md`; las específicas de lenguajes, tipos de archivo o tareas van en `*.instructions.md`.

## Repite el prompt y observa el cambio

1. En Copilot Chat, selecciona **New Chat**.
2. Vuelve a `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el mismo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo ahora incluye un encabezado parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa el comentario TSDoc propuesto para la función:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su identificador y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has dirigido a Copilot para que aplique un estándar nuevo al código que utilizarás en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba los cambios:

   ```bash
   git status
   ```

3. Prepara y confirma los archivos:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Copilot puede localizar la incidencia correspondiente, crear una solicitud de incorporación de cambios (PR) y combinarla.

> [!NOTE]
> La plantilla del proyecto ya conecta las herramientas de GitHub con tu espacio de trabajo. La primera vez que Copilot utilice una, VS Code podría pedirte que inicies sesión.

1. Abre Copilot Chat en el Codespace. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) y confirma que está seleccionado **Agent**.
2. Pide a Copilot que encuentre la incidencia y cree una PR desde la rama actual:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

3. Revisa cada acción que Copilot solicite y selecciona **Allow** cuando corresponda.
4. Cuando se cree la PR, pídele que la combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

5. Revisa las acciones de GitHub y del terminal antes de autorizarlas.

## Resumen y próximos pasos

Has observado cómo Copilot utiliza los archivos de instrucciones, repetido un prompt tras añadir un estándar general, confirmado la actualización y creado y combinado una PR con Copilot Chat. A continuación, aplicarás las instrucciones en [modo de agente][next-lesson] para añadir una función.

## Recursos

- [Personalizar GitHub Copilot con archivos de instrucciones][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para mejorar las instrucciones de Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales][personal-instructions]
- [Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
