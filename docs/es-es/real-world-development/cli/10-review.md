---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios y las personalizaciones reutilizables."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. Preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado; añadiste valoraciones por estrellas y combinaste manualmente tu primera solicitud de incorporación de cambios (PR); planificaste el filtrado con Plan, lo implementaste con Autopilot y lo revisaste en Interactive; y aplicaste instrucciones personalizadas, la habilidad `quality-checks`, Playwright MCP, un agente QA y Agent Merge. También exploraste comandos de barra para contexto, modelos, uso compartido y delegación opcional a la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones por estrellas:** muestra la propiedad existente `starRating` y un estado explícito para los juegos sin valoración.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil QA e incluye las pruebas relacionadas.

Desde la planificación del filtrado hasta la apertura de su PR, trabajaste en la misma conversación y rama. Se agrupó ese trabajo en una PR para simplificar el taller.

## Diferentes tipos de verificación

Comprobaste el código de varias formas: pruebas automatizadas, inspección propia en el navegador y exploración del navegador por Copilot mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos con el nuevo formato de informe. QA reunió estos resultados con la revisión de requisitos y cobertura antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; puede ser correcto que una ejecución de QA no necesite añadir pruebas. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas de verificación después de cualquier cambio.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil QA y configuraste un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y adáptalas a las necesidades del equipo. Las instrucciones fijan estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados. Usa **Plan** para acordar un enfoque, **Interactive** para mantener el control de cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo rápido para ediciones rutinarias y uno más capaz para trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad qué quieres crear, por qué y cómo influye significativamente en el resultado.

## Más opciones para explorar

- `/review` pide al agente de revisión de código que analice los cambios.
- `/rubber-duck` permite explicar un problema y obtener otra perspectiva.
- `/fleet` coordina subtareas independientes en paralelo.
- `/worktree` aísla otra tarea.
- `/delegate` envía una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola: en código de producción, en proyectos personales o en esa pequeña aplicación que hace tiempo que quieres crear. Comparte lo que aprendas con el equipo y consulta la documentación.

Para ampliar Tailspin Toys desde el terminal, sigue la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente de Copilot en la nube][cloud].

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
