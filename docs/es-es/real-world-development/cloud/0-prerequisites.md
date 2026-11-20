---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente de GitHub Copilot en la nube, prepara el entorno. Crearás una copia propia del repositorio Tailspin Toys e iniciarás un [Codespace][codespaces] para editar archivos de instrucciones y revisar el trabajo del agente.

## Configura el repositorio del taller

Crearás una instancia del [repositorio de plantilla][template-repository]. Incluirá todos los archivos necesarios para el taller.

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, puedes crear el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración del repositorio de plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio (`organization-or-user-name/repository-name`); la necesitarás más adelante.

## Crea un Codespace

Usarás un Codespace para completar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código en el navegador; ofrece un IDE completo con compatibilidad con varios lenguajes, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, **+** para crear un Codespace.

    ![Crear un Codespace](../../../_images/ex0-create-codespace.png)

La creación puede tardar varios minutos, aunque es mucho más rápida que instalar los servicios manualmente. Mientras esperas, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller puede realizarse en Codespaces o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen los requisitos necesarios. Para trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca la opción.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumen

¡Has creado una copia del repositorio del taller! También has iniciado la creación del Codespace que usarás mientras trabajas con el agente de Copilot en la nube.

## Próximo paso

Añadiremos instrucciones personalizadas que seguirá el agente. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson].

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Inicio rápido de Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
