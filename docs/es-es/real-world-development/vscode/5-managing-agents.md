---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición en tiempo real. Puedes revisar las diferencias, aceptar o rechazar cambios y dirigir la conversación con prompts de seguimiento.

En este ejercicio supervisarás el trabajo del agente de accesibilidad, revisarás las ediciones propuestas y le pedirás que añada un modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita un modo claro para quienes encuentran demasiado oscuro el tema predeterminado. En lugar de empezar de nuevo, ampliarás la sesión existente.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra los archivos explorados, los comandos ejecutados y los cambios propuestos. Cada edición aparece con una diferencia y controles **Keep** y **Undo**.

1. Vuelve a Copilot Chat desde el ejercicio 4.
2. Observa las llamadas a herramientas y las ediciones a medida que aparecen.
3. Para cada cambio, selecciona **Keep** para aceptarlo o **Undo** para revertirlo y pedir otro enfoque.
4. Abre el indicador **Files changed** para ver todos los archivos modificados.

> [!NOTE]
> Si una herramienta pide permiso, por ejemplo, para ejecutar un comando del terminal, Copilot mostrará **Allow**. Autoriza solo los comandos que consideres seguros.

5. Espera a que el agente termine el modo de alto contraste y revisa su resumen.

## Pedir al agente que añada el modo claro

La misma conversación conserva el contexto del trabajo anterior, incluidos los componentes, los estilos y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento:

   ```plaintext
   Buen trabajo. Ahora añade un control para el modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: guardar la preferencia en el almacenamiento local y permitir el acceso desde la misma zona de la interfaz. Cada persona debe poder activar el alto contraste y el modo claro de forma independiente.
   ```

2. Revisa los nuevos cambios y usa **Keep** o **Undo** en cada diferencia.
3. Si el agente toma un rumbo no deseado, detenlo con **Stop** y envía una aclaración.

> [!TIP]
> Dirigir una sesión en curso suele ser más rápido que empezar desde cero. Puedes pedir ajustes, corregir el enfoque o ampliar el alcance en prompts de seguimiento.

## Revisar el conjunto de cambios

1. Abre **Source Control** en VS Code y revisa todos los archivos modificados.
2. Inspecciona algunos cambios en los componentes Astro, los estilos y las pruebas.
3. Confirma que se aplican las prácticas de accesibilidad: atributos ARIA, navegación por teclado, HTML semántico y persistencia en almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en la [siguiente lección][next-lesson].

## Resumen y próximos pasos

Has supervisado una sesión en curso, revisado y aceptado o rechazado cambios individuales y ampliado el trabajo con prompts de seguimiento. En la [siguiente lección][next-lesson] publicarás los cambios de accesibilidad en una PR y los revisarás.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
