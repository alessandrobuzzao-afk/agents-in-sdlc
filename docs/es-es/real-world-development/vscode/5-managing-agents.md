---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando GitHub Copilot está en modo de agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu máquina en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones. Puedes revisar cada diff, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- pedirás al agente que añada un control para el modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro. En vez de empezar otra conversación, guiarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a Copilot Chat desde el Ejercicio 4.
2. Desplázate por el chat mientras trabaja para ver las llamadas a herramientas, lecturas, ediciones y comandos del terminal.
3. En cada edición, selecciona **Keep** para aceptarla o **Undo** para revertirla.
4. Selecciona **Files changed** en la parte superior para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará **Allow**. Autoriza solo los comandos que hayas revisado.

5. Espera a que termine el modo de alto contraste. Copilot mostrará un resumen de los cambios.

## Pide al agente que añada el modo claro

Continúa la misma conversación para que el agente conserve el contexto de los componentes, estilos y el uso de almacenamiento local.

1. En Copilot Chat, envía este prompt:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente revisará sus cambios anteriores y propondrá las adiciones del modo claro.
3. Revisa cada edición y selecciona **Keep** o **Undo**.
4. Si el agente sigue un enfoque que no quieres, puedes detenerlo con **Stop** y enviar una aclaración.

> [!TIP]
> Guiar una sesión en curso puede ser más rápido que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes, corregir el enfoque o ampliar el alcance; por ejemplo, sustituir selectores duplicados por variables CSS o integrar el modo claro en el panel de ajustes existente.

## Revisa los cambios

Antes de confirmar el trabajo, revisa los archivos modificados:

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos; debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y comprueba los atributos ARIA, la navegación por teclado, el HTML semántico y la persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has supervisado y guiado una sesión en curso de Copilot Chat: revisaste cada edición, aceptaste o rechazaste cambios y enviaste prompts de seguimiento sin perder el contexto.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que generó el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
