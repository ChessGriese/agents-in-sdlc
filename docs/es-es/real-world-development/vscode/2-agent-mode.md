---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso un cambio sencillo suele requerir modificar varios archivos y ejecutar pruebas. El modo agente de Copilot puede explorar la estructura del proyecto, coordinar los cambios necesarios, ejecutar tareas como las pruebas y corregir los errores que encuentre.

> [!NOTE]
> El modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en tu IDE, con ciclos de respuesta rápidos; el agente en la nube trabaja de forma asíncrona como un miembro del equipo, mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio usarás el modo agente para explorar el proyecto, implementar cambios coordinados en la interfaz y la capa de datos y revisar el código y las pruebas antes de combinar los cambios.

## Escenario

El catálogo de juegos crece y quieres que las personas puedan filtrar por categoría y editorial. En el ejercicio anterior añadiste una función auxiliar para las editoriales; ahora completarás la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio de Tailspin Toys

La aplicación es un único proyecto Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend separada ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con pequeños bloques `<script>` de Astro que usan las API DOM estándar.

1. Vuelve al Codespace y abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

3. Inicia el sitio:

   ```bash
   npm run dev
   ```

4. Abre `http://localhost:4321` manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la URL en el terminal. En Codespaces, se abrirá un túnel privado al puerto `4321`.
5. Explora la página principal, que muestra juegos, y selecciona un juego para ver la página de detalles con su título, descripción, editorial y categoría.

## Explorar el backlog con Copilot

> [!TIP]
> Antes de empezar, abre Copilot Chat en el Codespace y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo pueden variar en cada ejercicio.

1. Selecciona **Agent** en el selector de agentes de la vista Chat.

   ![Selector de agentes de la vista Chat con Agent seleccionado](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.
3. Pide a Copilot que muestre y priorice el backlog:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias y revisa la lista.

> [!CAUTION]
> No se pretende recomendar un modelo concreto. El taller se desarrolló con Claude Sonnet 4.5 para mantener resultados relativamente coherentes, pero las respuestas pueden variar porque los modelos son probabilísticos.

> [!NOTE]
> Copilot puede utilizar otro comando MCP, pero debería completar la tarea igualmente.

## Revisar los archivos de instrucciones

Antes de generar código, revisa `.github/instructions/ui.instructions.md`, que contiene contexto para añadir funcionalidades al sitio:

1. Revisa la arquitectura y las directrices para diseño de componentes, pruebas y accesibilidad.
2. Observa los enlaces a las instrucciones de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos y ejemplos del proyecto. Sus rutas son relativas al archivo de instrucciones, lo que permite reutilizar las directrices y dividirlas en secciones manejables.

## Implementar el filtrado

Completar el filtrado requiere actualizar la lógica de `src/lib/`, añadir o ajustar pruebas y modificar la página de listado para incorporar la interfaz de filtrado.

1. Continúa en la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y `ui`.

   ![Selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado y envía este prompt:

   ```plaintext
   Actualiza el sitio para permitir el filtrado por editorial y categoría según los requisitos de la incidencia de GitHub relacionada en el backlog. Ya existe una función auxiliar de editoriales del ejercicio anterior; consérvala y ajústala cuando sea necesario para completar la capa de datos, la interfaz y las pruebas restantes. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución; no hace falta iniciarlo.
   ```

4. Observa cómo Copilot localiza las funciones auxiliares, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

5. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas. Puede haber pausas o fallos temporales mientras corrige los cambios.

   ![Copilot Chat solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

6. Revisa el código generado y confirma que la funcionalidad se comporta como esperas en el navegador.
7. Cuando hayas revisado todo, selecciona **Keep** en Copilot Chat.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

## Resumen y próximos pasos

Has usado el modo agente para explorar el proyecto, identificar los archivos pertinentes y añadir una funcionalidad en la interfaz y la capa de datos. A continuación, [probarás el filtrado con Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

Como ejercicio adicional, puedes pedir a Copilot que implemente la paginación. Decide si el tamaño de página será fijo o configurable, utiliza los archivos de instrucciones existentes y proporciona el contexto necesario. Iterar con Copilot es parte habitual del proceso.

## Recursos

- [Modos ask, edit y agent de Copilot][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
