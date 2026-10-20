---
title: "Lección 7 - Crear y usar un agente de control de calidad"
description: "Crea un agente QA que reúna los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora reunirás estas capacidades en un agente personalizado con un proceso de control de calidad definido.

En esta lección explorarás cómo un agente personalizado combina instrucciones, habilidades y herramientas MCP; crearás un perfil reutilizable de control de calidad (QA), y lo seleccionarás para revisar los resultados según la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar este proceso y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida mediante un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller crearás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Cada personalización cumple una función distinta: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas para el navegador; y el perfil QA indica a Copilot cómo utilizar esas capacidades para evaluar requisitos e informar de los resultados. El perfil no sustituye estas herramientas ni requiere otra conversación.

## Crear el perfil de QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil QA reutilizable, con comprobaciones y límites bien definidos.

1. Vuelve al Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el perfil:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe informar de cada requisito como aprobado, fallido o bloqueado e incluir pruebas que lo respalden. Debe preguntar antes de modificar el código de implementación y no debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   De momento, crea solo el perfil para que pueda revisarlo.
   ```

## Inspeccionar el perfil

Antes de usar el agente, comprueba que el perfil incluye el proceso y los límites previstos:

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Revisa el frontmatter. `description` es obligatorio; `name` es opcional, pero ayuda a identificar el agente.
3. Comprueba que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta `quality-checks` y usa Playwright MCP.
4. Confirma que el informe incluye pruebas, que pregunta antes de modificar la implementación y que no crea commits ni PR.
5. Si falta algún requisito, pide al agente predeterminado que actualice el perfil.

## Ejecutar QA para la incidencia

Copilot CLI carga los agentes del proyecto al comenzar una conversación. Reanuda la conversación de filtrado para conservar la incidencia y las decisiones de planificación.

1. Activa el agente:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Es posible que el agente recién creado no aparezca en la lista, pero el comando anterior lo activará.

2. Pídele que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones del plan. ¿Está lista para una PR?
   ```

3. Cuando termine, lee el informe.

## Resumen y próximos pasos

Has añadido un rol especializado reutilizable y revisado su trabajo. La implementación, la actualización de la habilidad, el perfil QA, las pruebas y el informe de verificación están listos para revisarse. A continuación, [los reunirás en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
