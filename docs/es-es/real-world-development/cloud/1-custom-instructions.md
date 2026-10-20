---
title: "Ejercicio 1 - Instrucciones personalizadas (Agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental para la IA generativa. Los [archivos de instrucciones][instruction-files] proporcionan a Copilot orientación sobre qué debe hacer y cómo debe hacerlo.

En este ejercicio explorarás cómo las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas aportan contexto, directrices de programación y estándares de documentación al agente. Después, añadirás un estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás prompts de antes y después: el agente en la nube trabaja de forma asíncrona en incidencias de GitHub. Observarás el efecto de las instrucciones al revisar las PR que genere más adelante.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias directrices de desarrollo: la capa de datos debe contar con pruebas unitarias; la interfaz debe usar un tema oscuro y moderno; el código debe documentarse con comentarios TSDoc; y cada archivo debe comenzar con un bloque que explique su propósito. Las instrucciones ayudarán a Copilot a respetar estas prácticas.

### Instrucciones personalizadas

Las instrucciones permiten compartir con Copilot preferencias de estilo y requisitos del proyecto.

- `.github/copilot-instructions.md` es el archivo único que se envía a Copilot en todas las solicitudes del repositorio y debe incluir contexto general del proyecto.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni sugerir la siguiente edición. Copilot Chat, CLI y el agente en la nube utilizan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo`. Copilot también admite otros formatos, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas

- Mantén `copilot-instructions.md` centrado en el contexto general: proyecto, estructura y estándares globales.
- Usa `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos cuando ayuden.

> [!TIP]
> Explora los archivos de ejemplo, como las instrucciones para [interfaz][ui-instructions] y [Astro][astro-instructions]. También puedes usar Copilot para crear archivos de instrucciones o consultar [awesome-copilot][awesome-copilot] como punto de partida.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones del proyecto

Abre los archivos en la interfaz web de GitHub o en el Codespace:

1. Abre `.github/copilot-instructions.md` y revisa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, revisa las directrices de GitHub Actions.
2. Explora `.github/instructions` y los archivos para Astro, Drizzle, pruebas y otras tareas.
3. Abre `.github/instructions/unit-tests.instructions.md` y revisa `applyTo`, que determina los archivos a los que se aplican las instrucciones.
4. Revisa las directrices para pruebas unitarias y los enlaces del final de `.github/instructions/drizzle.instructions.md`.

> [!NOTE]
> La sección **Code formatting requirements** todavía no exige comentarios de documentación dentro del código. Añadirás esa norma a continuación.

## Añadir un estándar al repositorio

1. En el terminal del Codespace, crea y cambia a una rama:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abre `.github/copilot-instructions.md` y localiza **Code formatting requirements**.
3. Añade estas reglas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda el archivo.

## Confirmar, enviar y combinar la actualización

El agente en la nube lee las instrucciones de la rama a la que apunta la incidencia. Para que las use, los cambios deben estar en `main`.

1. Prepara y confirma el archivo:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. Si prefieres trabajar directamente en `main`, puedes omitir la rama.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza las instrucciones y has añadido un estándar para todo el repositorio. En la siguiente lección [asignarás tu primera incidencia al agente en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones][copilot-instructions-five-tips]
- [Awesome Copilot: instrucciones y recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
