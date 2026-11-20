---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios (PR). Combinaste manualmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, todo el flujo de filtrado y calidad se encuentra en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionarás el cambio completo de la funcionalidad y las evidencias de verificación;
- crearás la PR del filtrado;
- habilitarás Agent Merge solo después de revisarla y confirmarás que se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una PR. Cuando se habilita, Copilot resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar la base cuando es necesario. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

1. Vuelve a tu Codespace.
2. Abre el diálogo de agentes escribiendo `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y la combinará cuando se superen todas las comprobaciones.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del desarrollo: generación de código, pruebas y validación, y ahora el proceso de la PR. Has aprendido qué hace Agent Merge, inspeccionado el cambio completo, creado la PR de filtrado y confirmado que se ha combinado después de activar Agent Merge tras la revisión.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar contexto, modelos, uso compartido y delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
