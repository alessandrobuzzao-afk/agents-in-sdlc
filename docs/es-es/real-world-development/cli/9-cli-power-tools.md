---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente a la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Permiten utilizar funciones avanzadas, consultar información interna y cambiar la configuración. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Explorarás otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usarás `/model` para consultar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar a un agente en la nube.

## Escenario

Has terminado el flujo principal de CLI. Ahora explorarás otras capacidades: gestión del contexto, cambio de modelo, uso compartido de sesiones y delegación opcional al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas puedes acercarte al límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario; también puedes inspeccionarla o compactarla con comandos.

1. Vuelve a tu Codespace y abre Copilot CLI desde la raíz del repositorio, si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es pequeña, quizá no notes una gran diferencia.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` al cambiar a una tarea no relacionada y empezar una conversación nueva.

## Elige el modelo

Los modelos tienen diferentes puntos fuertes y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar los modelos disponibles y elegir uno.

1. Escribe `/model`.
2. Explora los modelos y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y los aprendizajes ayuda al equipo. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El taller principal ha terminado. Copilot CLI también dispone de comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una PR.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Empieza por una tarea nueva y bien definida, y revisa el resultado con tu flujo habitual. Para aprender más sobre el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI permiten cambiar su configuración, compartir sesiones y consultar su funcionamiento. Usaste `/context`, `/compact`, `/model` y `/share`, y exploraste comandos opcionales para trabajar en paralelo, crear árboles de trabajo y delegar tareas.

Hay más comandos de barra por explorar. Para terminar el recorrido, [repasarás lo aprendido y los próximos pasos][next-lesson].

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
