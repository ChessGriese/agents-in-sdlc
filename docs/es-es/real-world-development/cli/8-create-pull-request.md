---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Ahora los revisarás en conjunto y abrirás una solicitud de incorporación de cambios. La PR de las valoraciones por estrellas la combinaste manualmente; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la habilidad y el agente QA en varias PR. Para simplificar el taller, todo el flujo de filtrado y calidad se ha mantenido en una conversación y una rama.

En esta lección conocerás Agent Merge, inspeccionarás el cambio completo y sus pruebas de verificación, crearás la PR del filtrado y activarás Agent Merge solo después de revisar el trabajo.

## Escenario

Has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener la autorización de combinación bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo pendiente para combinar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR —por ejemplo, corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y actualiza la base— y activa la combinación automática de GitHub cuando el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

1. Vuelve al Codespace.
2. Abre el diálogo de agentes con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y, cuando todo se complete correctamente, la combinará.
7. Confirma que Copilot informa de que la PR se ha combinado correctamente, por ejemplo: «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no evita las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, revisa el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del desarrollo: generación de código, pruebas y validación, y ahora el proceso de solicitud de incorporación de cambios. A continuación explorarás [otros comandos de barra útiles de Copilot CLI][next-lesson].

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
