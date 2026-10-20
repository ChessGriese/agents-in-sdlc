---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys, iniciarás un [Codespace][codespaces] y confirmarás que Copilot Chat funciona en el editor.

## Configurar el repositorio del taller

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las indicaciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

   ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio (`organization-or-user-name/repository-name`) para consultarla más adelante.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con esas incidencias durante el taller; no tienes que crear ninguna.

## Crear un Codespace

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube para escribir, ejecutar y depurar código en el navegador. Proporciona un IDE con compatibilidad para varios lenguajes, extensiones y herramientas.

1. Ve al repositorio recién creado y selecciona **Code**.
2. Selecciona la pestaña **Codespaces** y el botón **+** para crear un Codespace.
3. Espera unos minutos a que termine la creación. Mientras tanto, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. De momento, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller funciona en Codespaces o en un [contenedor de desarrollo][dev-containers] local. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite.

## Usar Copilot Chat y el modo agente

Para usar el modo agente de Copilot Chat, necesitas la extensión GitHub Copilot Chat, que ya debería estar instalada en Codespaces.

> [!TIP]
> Si no tienes la extensión, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension] o búscala en la vista Extensions de VS Code y selecciona **Install**.

1. Vuelve al Codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat**.
3. Escribe un mensaje, como «Hola», y pulsa <kbd>Enter</kbd>.
4. Si se te solicita, inicia sesión con tu cuenta de GitHub y sigue las instrucciones.
5. Comprueba que aparece la ventana Copilot Chat.

   ![Prompt de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

## Resumen y siguiente paso

Has creado una copia del repositorio, iniciado un Codespace y confirmado que Copilot Chat está disponible. A continuación, [añadirás instrucciones personalizadas][next-lesson] para enseñar a Copilot las convenciones del proyecto.

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
