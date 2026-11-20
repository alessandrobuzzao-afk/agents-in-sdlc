---
title: "Lección 10 - Recapitulación y próximos pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo continuo de Tailspin Toys. Has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado; añadido valoraciones con estrellas y combinado manualmente la primera solicitud de incorporación de cambios (PR); planificado e implementado el filtrado con Plan y Autopilot; y revisado el resultado en Interactive. También has actualizado instrucciones, personalizado `quality-checks`, usado Playwright MCP, creado un agente personalizado de QA, revisado el cambio completo y autorizado Agent Merge. Por último, has explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones con estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y aplícalas a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, usaste la misma conversación y rama. Agrupamos ese trabajo en una PR para simplificar el taller.

## Diferentes tipos de verificación

Comprobaste el código de varias formas: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador de Copilot a través de MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y las comprobaciones de tipos; QA reunió esos resultados con una revisión de requisitos y cobertura antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; puede ser correcto que una ejecución de QA no requiera pruebas adicionales. Revisa el código y las evidencias antes de autorizar la combinación y actualiza las evidencias afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En el taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Puedes reutilizar estas personalizaciones entre conversaciones y adaptarlas a las necesidades del equipo. Las instrucciones definen estándares; las habilidades describen tareas repetibles; los agentes personalizados definen roles especializados; y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Usa **Plan** para acordar un enfoque, **Interactive** para colaborar en cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas por explorar

Has cubierto el flujo principal. También puedes probar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola: para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte tus aprendizajes con el equipo y aprovecha los de los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
