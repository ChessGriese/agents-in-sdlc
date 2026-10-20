---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores, asignaste al agente de Copilot en la nube varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante supervisar su progreso. La [página de agentes][agents-page] de GitHub.com funciona como centro de control: muestra las tareas con PR abiertas, las operaciones realizadas y las sesiones que puedes dirigir mientras siguen en curso.

En este ejercicio explorarás la página de agentes y dirigirás una sesión activa para pedir una funcionalidad adicional.

## Escenario

Después de solicitar un modo de alto contraste, el equipo decidió añadir también un modo claro. Como el agente ya está actualizando los estilos y añadiendo controles, ampliarás el alcance de su trabajo para incluir el nuevo modo.

## Revisar las tareas del agente en la nube

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas de los paneles principal y lateral. Deberías ver las incidencias que asignaste: documentación del código, juegos relacionados y modo de alto contraste.
3. Selecciona una tarea activa y revisa las operaciones realizadas, como obtener el código del repositorio, configurar el entorno, preparar servidores MCP, generar API para modificar juegos y completar la tarea.

> [!NOTE]
> Los pasos concretos dependen del estado de la tarea y del enfoque que haya seguido Copilot.

4. Revisa también el panel de la PR, que muestra los archivos modificados y permite seguir el trabajo.

## Dirigir al agente en la nube

1. Selecciona la sesión del modo de alto contraste. El título exacto puede variar.

   ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera hasta que el agente haya terminado de configurar el entorno y haya empezado a trabajar.
3. En el diálogo **Steer active session while Copilot is working**, envía este prompt:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo para que cada persona pueda seleccionar la apariencia que prefiera.
   ```

   ![Tarea del agente en la nube con el diálogo para dirigir una sesión activa](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt y observa cómo Copilot lo incorpora al trabajo.

El agente añadirá la nueva solicitud después de terminar el paso en curso. Puede tardar varios minutos.

## Resumen y próximos pasos

Has usado la página de agentes como centro de control para supervisar tareas y dirigir una sesión activa, solicitando funcionalidad adicional. A continuación, [iterarás sobre las PR creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
