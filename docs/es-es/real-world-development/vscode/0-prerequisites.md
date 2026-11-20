---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara el entorno. Crearás una copia propia del repositorio Tailspin Toys, iniciarás un [Codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del taller

Crearás una instancia a partir de la [plantilla][template-repository], que incluirá los archivos necesarios para el taller.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración del repositorio de plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta que has creado (`organization-or-user-name/repository-name`); la necesitarás más adelante.

> [!NOTE]
> **El backlog está preparado**
>
> Al crear el repositorio desde la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con ellas durante el taller; no tienes que crear ninguna.

## Crea un Codespace

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código en el navegador. Ofrece un IDE completo con compatibilidad con varios lenguajes, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, **+** para crear un Codespace.

    ![Crear un Codespace](../../../_images/ex0-create-codespace.png)

La creación puede tardar varios minutos, pero es más rápida que instalar los servicios manualmente. Mientras esperas, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al Codespace en un ejercicio futuro. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> El taller puede realizarse en Codespaces o en un [contenedor de desarrollo][dev-containers] local. Ambos incluyen los requisitos necesarios. Para trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca la opción.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usa GitHub Copilot Chat y el modo de agente

Para acceder al modo de agente de GitHub Copilot Chat, necesitas la extensión GitHub Copilot Chat, que ya debería estar instalada en un Codespace.

> [!TIP]
> Si no está instalada, puedes [instalarla desde Visual Studio Code Marketplace][copilot-chat-extension]. También puedes abrir la vista **Extensions**, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Es posible que tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve a tu Codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona su icono en la parte superior del Codespace.
3. Escribe «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>.
4. Si se te solicita, sigue las instrucciones para iniciar sesión en tu cuenta de GitHub.

    ![Prompt de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, aparecerá la ventana de Copilot Chat.

## Resumen

Has creado una copia del repositorio del taller e iniciado la creación del Codespace que utilizarás para programar.

## Próximo paso

Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson], donde enseñarás a Copilot las convenciones del proyecto.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
