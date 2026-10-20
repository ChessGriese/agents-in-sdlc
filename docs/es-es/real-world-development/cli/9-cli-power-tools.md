---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos para gestionar el contexto, elegir modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras herramientas CLI, GitHub Copilot CLI incluye comandos de barra para acceder a funciones avanzadas, información interna y opciones de configuración. Ya has usado `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. En esta lección explorarás otros comandos útiles.

Aprenderás a usar `/context` y `/compact` para consultar el contexto de la conversación, `/model` para explorar los modelos disponibles y `/share` para exportar o compartir sesiones. También conocerás comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo principal de CLI. Ahora revisarás otras funciones: gestión del contexto, selección de modelos, uso compartido de sesiones y delegación opcional al [agente de Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas largas o complejas podrías alcanzar el límite de contexto del modelo. Copilot CLI lo compacta automáticamente cuando es necesario; también puedes consultarlo o compactarlo con comandos de barra.

1. Vuelve al Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe `/context` y revisa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
3. Escribe `/compact` para compactar la conversación.
4. Vuelve a escribir `/context` y compara los resultados. Si la conversación ya era breve, quizá no haya grandes diferencias.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto a medida que se llena. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` al cambiar a una tarea distinta y quieras iniciar otra conversación.

## Elegir un modelo

Los modelos tienen distintas capacidades y cada desarrollador puede tener preferencias diferentes.

1. Escribe `/model`.
2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige un destino adecuado, como `/share file` para exportarla localmente a Markdown.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o la conversación que el equipo no quiera compartir.

## Opcional: ampliar o delegar el trabajo

El taller principal ya ha terminado. Copilot CLI también incluye comandos para tareas mayores:

- `/fleet` puede distribuir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una PR.

Estos comandos son opcionales porque pueden crear worktrees adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien delimitada, y revisa los resultados con tu proceso habitual. Para profundizar en el trabajo asíncrono, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Has utilizado comandos de barra para gestionar el contexto, explorar modelos, compartir sesiones y conocer opciones de trabajo en paralelo y delegación. Ahora [repasarás lo aprendido][next-lesson] y los próximos pasos.

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
