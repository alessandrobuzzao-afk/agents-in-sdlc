---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene proporcionarle ese contexto. Los [archivos de instrucciones][instruction-files] indican no solo *qué* quieres, sino también *cómo* quieres que se haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas;
- añadirás un estándar general al repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts antes y después: el agente en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que su impacto es más difícil de mostrar en tiempo real. Verás cómo influyen las instrucciones cuando revises las solicitudes que cree en ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias pautas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Mediante archivos de instrucciones, proporcionarás a Copilot el contexto necesario para que siga estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar a Copilot contexto y preferencias, como convenciones de programación, bibliotecas y comentarios. Se puede crear un archivo para todo el repositorio o archivos específicos para ciertos tipos de tarea.

- `.github/copilot-instructions.md` se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto y pautas pertinentes para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md` contiene pautas para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> En el IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para completar código ni sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot también admite otros formatos, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas

- Mantén `copilot-instructions.md` centrado en las pautas generales del proyecto, su estructura y los estándares globales.
- Usa `*.instructions.md` para pautas de tipos de archivo o tareas concretas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de cómo debería y no debería ser el código.

No hay una única manera correcta de crear instrucciones: experimenta y ajústalas a tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería contar con instrucciones adecuadas. En este repositorio encontrarás, entre otras, instrucciones para [cambios de interfaz][ui-instructions] y [Astro][astro-instructions]. Copilot también puede ayudar a crear archivos de instrucciones. Según el entorno, puedes usar **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. Para obtener plantillas, consulta [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Lee los archivos de instrucciones del repositorio en el editor o en la interfaz web de GitHub:

1. Abre `.github/copilot-instructions.md`.
2. Explora la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas para los flujos de trabajo de GitHub Actions.
3. Explora `.github/instructions`, donde hay instrucciones para Astro, Drizzle, pruebas y otras tareas.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa su campo `applyTo`, que determina los archivos aplicables; por ejemplo, archivos TypeScript que coincidan con `**/*.test.ts`.
5. Fíjate en las pautas específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y consulta sus enlaces a otras instrucciones y a archivos existentes. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Añade un estándar al repositorio

El siguiente paso es la única edición que harás aquí: añade una regla general para que la documentación se escriba en comentarios TSDoc y encabezados de archivo. El agente la utilizará cuando trabaje en incidencias posteriores.

Antes de editar, crea una rama:

1. En el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abre `.github/copilot-instructions.md`.
3. Debajo de los estándares existentes, añade:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda el archivo.

> [!TIP]
> Los archivos de instrucciones pueden aplicarse a todo el repositorio o a lenguajes, tipos de archivo y tareas concretos. `copilot-instructions.md` es el lugar adecuado para estándares generales como los comentarios de documentación.

## Confirma, publica y combina la actualización

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una en el siguiente ejercicio, partirá de `main`; por eso, tus instrucciones deben llegar a `main`.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. Así, el agente podrá usar estas instrucciones en el próximo ejercicio.

> [!TIP]
> También puedes trabajar directamente en `main` y omitir el paso de la rama; se incluye para reflejar un flujo habitual de trabajo.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que cree el agente durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para mejorar las instrucciones personalizadas de Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
