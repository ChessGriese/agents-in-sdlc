---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Has trabajado con GitHub Copilot en varias tareas: añadir documentación al código, crear una funcionalidad de juegos relacionados para el equipo de diseño e implementar funciones de accesibilidad con modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, darás feedback a Copilot.

Los fundamentos de diseño de software y DevOps siguen siendo los mismos con la IA generativa: revisa el código generado y sigue el proceso habitual del equipo. En esta lección revisarás las sugerencias para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente de Copilot en la nube

El agente trabaja de forma asíncrona y sin supervisión, por lo que existen medidas de seguridad:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura solo a la rama de trabajo.
- El agente se ejecuta en un entorno efímero separado dentro de GitHub Actions.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- El acceso a recursos externos está limitado de forma predeterminada, incluidos los servidores MCP.

## Revisar la documentación generada

1. Abre el repositorio en github.com y selecciona **Pull Requests**.
2. Abre la PR que haya creado Copilot para añadir documentación, con un título similar a **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR mostrará la etiqueta **[WIP]**. Espera a que termine.

3. Selecciona **Files changed** y revisa el código y los comentarios TSDoc.
4. Vuelve a **Conversation**. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows** y comprueba que las verificaciones del proyecto se completen correctamente.

   ![Pestaña Files changed de la solicitud de incorporación de cambios](../../../_images/shared-pr-files-changed.png)

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

> [!NOTE]
> Puede aparecer un aviso de que el cortafuegos bloquea a Copilot. Es normal: el acceso a recursos externos está limitado. Puedes [configurar el cortafuegos][agent-firewall] si procede.

### Solicitar cambios a Copilot

Puedes mencionar a Copilot en un comentario de la PR o en una revisión de código para solicitar cambios. Por ejemplo, puedes pedir encabezados de comentario para los archivos, comentarios TSDoc en TypeScript y Astro o un README que describa la estructura de la aplicación.

1. Añade un comentario con `@copilot` y una solicitud de cambio.
2. Selecciona **View Session** para observar la nueva sesión y el trabajo de Copilot.
3. Cuando termine, vuelve a la PR, revisa el commit nuevo y los archivos modificados.
4. Sigue iterando hasta que estés conforme; después, convierte la PR preliminar en lista para revisión y combínala cuando proceda.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

![Convertir la PR preliminar en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

1. En **Pull Requests**, abre la PR para mostrar juegos relacionados en la página de detalles.
2. Revisa **Files changed** y vuelve a **Conversation**.
3. Si se solicita, selecciona **Approve and run workflows** y espera a que terminen las comprobaciones.
4. Opcionalmente, cambia a la rama de Copilot en el Codespace para probar la funcionalidad:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Puedes pedir cambios adicionales, por ejemplo, ajustar el número de juegos relacionados o añadir documentación. Añade un comentario nuevo en **Conversation** y Copilot iniciará otra sesión.

## Revisar las funciones de accesibilidad

La PR del agente de accesibilidad debe incluir el modo de alto contraste solicitado en el ejercicio 3 y el modo claro pedido durante la supervisión en el ejercicio 4.

1. Abre la PR de accesibilidad y revisa **Files changed**.
2. Comprueba los controles de modo, el almacenamiento local de preferencias, los cambios de estilo, los atributos ARIA, la navegación por teclado y el código que cambia la apariencia.
3. En **Conversation**, selecciona **Approve and run workflows** si hay comprobaciones pendientes y espera a que se completen.
4. Opcionalmente, cambia a la rama de Copilot en tu Codespace y prueba que ambos modos funcionan y que las preferencias se conservan al recargar.

Si detectas problemas de accesibilidad, menciona a `@copilot` en un comentario para pedir cambios.

## Ejercicio opcional: seguir delegando

Para practicar, crea más incidencias y asígnalas al agente en la nube. Algunas ideas:

- Crear un formulario para que los patrocinadores expresen su interés en una página de detalles.
- Implementar la paginación en la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen y próximos pasos

Has completado el itinerario del agente en la nube: revisaste las instrucciones, asignaste incidencias, utilizaste un agente de accesibilidad, supervisaste y dirigiste su sesión e iteraste sobre las PR de Copilot.

Para continuar, explora los talleres de [VS Code](../../vscode/) y [CLI](../../cli/), o prueba en tu repositorio una incidencia de refactorización, un agente para otro ámbito o un archivo `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas del flujo de configuración][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../4-managing-agents/
[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
