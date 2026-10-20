---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en el Codespace, familiarízate con el entorno y localiza la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Primero instalarás la herramienta.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación.
- localizarás la incidencia de filtrado con el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a usar agentes de IA para gestionar un backlog creciente. Copilot CLI lleva esa capacidad al terminal, donde muchos desarrolladores trabajan habitualmente. En esta lección lo instalarás, autenticarás y prepararás para el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. GitHub Codespaces ya incluye Node.js, así que usarás npm.

1. Vuelve al Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Debe aparecer la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Autenticarse con GitHub

La primera vez que lo inicies, Copilot CLI te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI mostrará este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde al repositorio Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un Codespace quizá ya tengas una sesión autenticada de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está listo.

## Familiarizarse con Copilot CLI

Los comandos que escribes en el terminal normal se ejecutan directamente en el Codespace. Una vez iniciado Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsarlo para confirmar.
2. Escribe `/help` para ver los comandos de tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de contestar.
5. Escribe `/mcp list` y confirma que el servidor MCP integrado de GitHub está disponible.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Usa GitHub MCP para buscar en este repositorio la incidencia titulada "Allow users to filter games by category and publisher". Dame su URL y un breve resumen. No hagas ningún cambio.
   ```

7. Abre la URL y lee la incidencia; la usarás después de realizar un primer cambio sencillo.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama activa del terminal; no crea automáticamente un worktree. Crearás una rama de funcionalidad antes de cada cambio.

## Usar el atajo del taller

Copilot CLI suele pedir autorización antes de usar herramientas fuera de los permisos establecidos. En este taller, volverás a iniciarlo con `--yolo`, un atajo aprobado por ti que elimina esas solicitudes dentro del Codespace.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual; no lo configures como opción predeterminada para el desarrollo diario.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de GitHub de lectura y escritura que usarán las lecciones posteriores. Aunque el Codespace limita el acceso a tu ordenador local, los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta sencilla sobre el proyecto, confirma que funciona y sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de modificar instrucciones o añadir un agente, usarás `copilot --resume` para volver a la misma conversación y rama.

## Resumen y próximos pasos

Has instalado GitHub Copilot CLI con npm, te has autenticado, has confiado en el repositorio del taller y has encontrado la incidencia de filtrado con el servidor MCP integrado de GitHub. A continuación, [harás un primer cambio concreto][next-lesson] para mostrar la valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
