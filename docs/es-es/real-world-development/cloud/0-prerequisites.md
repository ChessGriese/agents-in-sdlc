---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente de Copilot en la nube, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys y pondrás en marcha un [Codespace][codespaces] para editar archivos de instrucciones y revisar el trabajo del agente.

## Configurar el repositorio del taller

Crea una copia del repositorio a partir de la [plantilla][template-repository]:

1. En una nueva ventana del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.
3. Si participas en un evento de GitHub o Microsoft, sigue las indicaciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio (`organization-or-user-name/repository-name`) para consultarla más adelante.

## Crear un Codespace

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube para escribir, ejecutar y depurar código en el navegador.

1. Ve al repositorio que acabas de crear y selecciona **Code**.
2. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.
3. Espera unos minutos a que termine la configuración. Puedes aprovechar ese tiempo para explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al Codespace en una lección posterior. De momento, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un Codespace o en un [contenedor de desarrollo][dev-containers] local. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite.

## Resumen y siguiente paso

Has creado una copia del repositorio e iniciado la creación del Codespace que usarás con el agente de Copilot en la nube. A continuación, [añadirás instrucciones personalizadas][next-lesson].

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
