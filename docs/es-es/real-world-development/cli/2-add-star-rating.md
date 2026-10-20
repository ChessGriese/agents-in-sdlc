---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para modificar las tarjetas de juegos, revisar el cambio en un navegador reenviado y combinarlo como tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero las tarjetas de la página principal aún no la muestran. Pedirás al agente que la añada, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación centrada en una rama de funcionalidad.
- pedirás al agente un cambio pequeño.
- revisarás los cambios con `/diff`.
- ejecutarás la aplicación y comprobarás el cambio en el navegador reenviado.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que tengas abiertos en el terminal, en lugar de crear un worktree dedicado. Las conversaciones guardadas permiten volver a la misma discusión; los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás los prompts y las respuestas del agente, la actividad de sus herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y pedir el cambio

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está abierto, inicia la herramienta con:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt:

   ```plaintext
   Muestra la propiedad starRating de cada juego sobre 5 en las tarjetas de la página de lista de juegos. Si la valoración es null, muestra "Aún sin valoración". Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario.

## Revisar las diferencias

Revisa todos los cambios generados por IA antes de combinarlos, aunque sean pequeños.

1. Escribe `/diff` e inspecciona cada archivo modificado.
2. Confirma que las tarjetas muestran la valoración numérica cuando existe y «Aún sin valoración» cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija cualquier fallo.
5. Cuando termines, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, produce resultados probabilísticos, no deterministas. El código exacto puede variar; revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprobar los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para examinar la interfaz en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de las valoraciones por estrellas. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abrir y combinar tu primera solicitud de incorporación de cambios

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de las valoraciones por estrellas con un mensaje de commit adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI enviará el commit existente al crear la solicitud de incorporación de cambios (PR) y mostrará su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la URL.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté lista, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve al Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y próximos pasos

Has publicado tu primer cambio con GitHub Copilot CLI. Iniciaste una conversación centrada en una rama, pediste un cambio pequeño en las tarjetas, lo revisaste con `/diff`, lo comprobaste en el navegador reenviado y abriste y combinaste tu primera PR.

Ahora partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
