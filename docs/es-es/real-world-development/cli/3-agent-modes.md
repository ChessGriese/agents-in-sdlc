---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos con una pequeña funcionalidad. Para cambios mayores hace falta un proceso más sólido. GitHub Copilot CLI se integra con el flujo de trabajo existente de una organización para ayudar a desarrollar las soluciones adecuadas. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partir de una incidencia, crear una funcionalidad, verificar el código y su comportamiento y, finalmente, combinarla con el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama durante todo el flujo de trabajo de la funcionalidad. Normalmente crearías ramas o PR distintas para cada tipo de archivo, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección iniciarás una conversación desde una incidencia de GitHub, definirás los requisitos en modo Plan, implementarás la funcionalidad en modo Autopilot, revisarás el código y validarás manualmente la funcionalidad en el navegador reenviado. Más adelante actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación MCP, crearás un agente de control de calidad y abrirás la PR.

## Escenario

El catálogo de Tailspin Toys crece y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para resolver estas decisiones y autorizar después una implementación acotada con Autopilot.

## Contexto

La introducción de agentes de programación con IA no cambia los fundamentos del desarrollo. La mayoría de los equipos siguen un proceso parecido:

1. Partir de una incidencia que describa el trabajo.
2. Planificar lo que se va a crear.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas.
5. Validar manualmente la funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de su revisión y de que se completen correctamente las comprobaciones de integración continua.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero suelen seguir una secuencia similar.

## Modos de conversación

El **modo de conversación** determina el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente; este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente prepara un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Partir de la incidencia

1. Vuelve al Codespace y, si está detenido, reinícialo.
2. Confirma que se ha combinado la PR de las valoraciones por estrellas.
3. Abre el terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Con las flechas, selecciona **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

El prompt empieza ahora por `#7` o un número similar. El prefijo `#` añade al contexto una incidencia o PR de GitHub.

## Planificar la funcionalidad de filtrado

La planificación permite definir el enfoque antes de que Copilot implemente una funcionalidad o realice una tarea.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan y confirma que el indicador muestra **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot explorará el proyecto y preparará el plan.

3. Puede que Copilot te pregunte cómo debe funcionar el filtrado. Responde según tus preferencias; no hay una única respuesta correcta.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Revisa el plan y pide que se modifiquen las partes que no reflejen tus decisiones.

## Aprobar Autopilot

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador de modo muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente; revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisar y verificar la implementación

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en modo Interactive y confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones de planificación.
4. Pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija cualquier fallo.

## Probar la nueva funcionalidad

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la funcionalidad de filtrado en el navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones que acordaste en el plan.
4. Confirma que el restablecimiento y los resultados vacíos coinciden con la incidencia y tus decisiones.
5. Pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y próximos pasos

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad: partiste de una incidencia, definiste los requisitos en Plan, implementaste con Autopilot, revisaste el código y validaste el comportamiento en el navegador.

Ahora profundizarás en cómo se genera el código y cómo respetar las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
