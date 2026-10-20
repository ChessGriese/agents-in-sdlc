---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Muchas organizaciones acumulan deuda técnica: problemas de seguridad sin resolver, código heredado que requiere actualización o solicitudes que llevan tiempo en el backlog. El agente de GitHub Copilot en la nube puede abordar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una PR preliminar para que la revise una persona. Así puedes delegar tareas mientras te centras en otros asuntos.

En este ejercicio personalizarás el entorno de generación de código, revisarás las medidas de seguridad, aprenderás la importancia de delimitar bien las incidencias y asignarás tareas a Copilot.

## Escenario

Tailspin Toys quiere reducir la deuda técnica. Primero, el equipo necesita comentarios TSDoc en las funciones exportadas. Además, el equipo de diseño quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para probar la experiencia de usuario.

Son tareas que a menudo se posponen y que puedes asignar al agente en la nube para que trabaje de forma asíncrona mientras tú te ocupas de otras actividades.

## Introducción al agente en la nube de GitHub Copilot

El [agente de GitHub Copilot en la nube][copilot-agents] trabaja en segundo plano como un desarrollador: puedes asignarle una [incidencia de GitHub][assign-issue] y creará una PR preliminar, configurará un entorno y empezará la tarea. Puedes consultar la sesión mientras trabaja o cuando haya terminado. Cuando esté lista para revisión, te mencionará en la PR.

## Delimitar bien las instrucciones

GitHub Copilot no elimina la necesidad de planificar. Incluso las operaciones aparentemente sencillas pueden ser complejas; aplica las [prácticas recomendadas para delimitar tareas][cloud-agent-best-practices]. Trata al agente como a un compañero de programación: trabaja por etapas, aprende, experimenta y adapta el enfoque.

## Instrucciones personalizadas de este repositorio

Antes de asignar trabajo, revisa estos archivos en la interfaz web o en el Codespace:

- `.github/copilot-instructions.md`: consulta **Code standards**, en especial las convenciones de TypeScript y TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa cómo `applyTo` limita las instrucciones a `**/*.test.ts`.

Al revisar la PR de la incidencia `Code lacks documentation`, comprueba si se aplican las convenciones de TypeScript, los comentarios TSDoc y los encabezados de archivo.

## Configurar el entorno del agente

El agente en la nube usa [GitHub Actions][github-actions]. El archivo `.github/workflows/copilot-setup-steps.yml` contiene un flujo de configuración que instala Node.js, las dependencias, los navegadores de Playwright y prepara la base de datos. El flujo ya está configurado en la plantilla. Incluye el trabajo `copilot-setup-steps` y el activador `workflow_dispatch`, que permite ejecutarlo manualmente desde **Actions** para comprobar la configuración.

El flujo de trabajo tiene este aspecto:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna solo los permisos mínimos que necesiten tus pasos. Copilot recibirá su propio token para las operaciones.
    permissions:
      # Para clonar el repositorio durante la configuración, por ejemplo para instalar dependencias, necesitas el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (toda la aplicación utiliza Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos en SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

Este archivo contiene un único trabajo, `copilot-setup-steps`, que GitHub Actions ejecuta antes de que Copilot empiece la PR. El activador `workflow_dispatch` permite probarlo manualmente en **Actions** en lugar de esperar a que lo ejecute Copilot.

## Asignar la tarea de documentación

1. Abre el repositorio en github.com y selecciona **Issues** y **New issue**.
2. Crea una incidencia en blanco titulada `Code lacks documentation`.
3. Usa esta descripción:

   ```plaintext
   Nuestra organización requiere que las funciones y los métodos incluyan comentarios TSDoc cuando ayuden a comprender el código. Las últimas actualizaciones no han seguido este estándar. Actualiza el código existente para añadir comentarios donde aclaren el comportamiento.
   ```

4. Selecciona **Create** y, en el panel derecho, **Assign to Copilot**.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

5. Selecciona **Assign**. Abre la pestaña **Pull Requests** y consulta la PR que genere Copilot.
6. Revisa la lista de tareas creada por Copilot y selecciona **View session** para seguir el trabajo.

   ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!NOTE]
> La lista de tareas puede tardar unos minutos en aparecer: Copilot está configurando el entorno, analizando el proyecto y decidiendo cómo abordar la tarea.

> [!CAUTION]
> Quizá tengas que actualizar la ventana para ver el indicador de sesión actualizado.

Mientras el agente trabaja, asigna una segunda incidencia:

1. Crea una incidencia en blanco titulada `Show related games on the game details page`.
2. Describe el requisito: añadir en `src/lib/` una función que devuelva otros juegos de la misma categoría y excluya el juego actual; mostrar una sección «Related games» que use esa función; gestionar el caso sin juegos relacionados; añadir pruebas unitarias y confirmar que todas se superan antes de crear la PR.
3. Selecciona **Create**, después **Assign to Copilot** y, por último, **Assign**.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

Verás el emoji 👀 en el primer comentario de la incidencia cuando Copilot empiece a trabajar. No hace falta supervisar constantemente el agente asíncrono; revisa el trabajo al terminar.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

## Resumen y próximos pasos

Has revisado el entorno y las medidas de seguridad del agente en la nube y le has asignado dos tareas: documentar el código y añadir juegos relacionados. A continuación, [crearás y utilizarás agentes personalizados][next-lesson].

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración][cloud-agent-best-practices]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
