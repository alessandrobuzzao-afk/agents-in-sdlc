---
title: "Lección 7 - Crea y utiliza un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora reunirás esas capacidades en un agente personalizado con un proceso de QA bien definido.

En esta lección:

- explorarás cómo colabora un agente personalizado con instrucciones, habilidades y herramientas MCP;
- crearás y revisarás un perfil reutilizable de control de calidad;
- seleccionarás el agente de QA y revisarás sus hallazgos respecto a la incidencia.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar el proceso y crear un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Cada personalización cumple una función distinta: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador; y el perfil de QA indica a Copilot cómo utilizarlas para evaluar los requisitos y comunicar los hallazgos.

## Crea el perfil de QA

Antes de abrir la PR, pide a Copilot que cree un perfil de QA reutilizable con las comprobaciones y los límites correspondientes.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el agente, revisa su perfil para confirmar que refleja el flujo de QA y los límites previstos.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero proporciona un nombre visible.
3. Confirma que el perfil empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta `quality-checks` y usa Playwright MCP.
4. Confirma que comunica pruebas con evidencias, pide permiso antes de cambiar código de implementación y no crea confirmaciones ni PR.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta QA respecto a la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Retoma la misma conversación de filtrado y selecciona QA para que tenga acceso a la incidencia y las decisiones de planificación.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Es posible que el agente recién creado no aparezca en la lista. El comando anterior lo activará igualmente.

2. Pide al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Espera a que termine el agente.
4. Lee el informe.

## Resumen y próximos pasos

Has incorporado un rol especializado reutilizable al flujo de trabajo y has revisado sus resultados. Creaste e inspeccionaste el perfil de QA, que combina instrucciones, habilidades y herramientas MCP, y revisaste sus hallazgos respecto a la incidencia.

Ahora tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación. A continuación, [los reunirás en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
