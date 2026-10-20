---
title: "Ejercicio 3 - Probar la funcionalidad con Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios (PR), debes comprobar que funciona en el navegador. Configurarás el **servidor MCP de Playwright** para que Copilot controle un navegador real y pruebe la funcionalidad. Después publicarás la rama y abrirás la PR.

En este ejercicio aprenderás qué es Model Context Protocol (MCP), cómo amplía Copilot con herramientas, cómo añadir Playwright MCP y cómo pedir a Copilot que pruebe el filtrado en un navegador.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite a los agentes de IA comunicarse con herramientas y servicios externos. Los servidores MCP actúan como puente entre el agente y las herramientas: GitHub MCP proporciona acceso a API para administrar repositorios, incidencias y PR; Playwright MCP permite automatizar un navegador. GitHub también mantiene un [registro MCP][mcp-registry].

![Diagrama del modo agente y su interacción con el contexto, el modelo de lenguaje y las herramientas, incluidas las que aportan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

> [!CAUTION]
> Trata los servidores MCP como cualquier dependencia. Revisa su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad antes de conceder acceso a recursos sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP del espacio de trabajo. Abre el archivo en el Codespace y comprueba que ya incluye el servidor `github`:

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

La plantilla incluye esta entrada, por eso Copilot pudo leer el backlog en el ejercicio anterior. El servidor remoto se conecta por HTTP y requiere autenticación de GitHub cuando Copilot usa sus herramientas por primera vez.

## Añadir el servidor Playwright MCP

Añade la entrada `playwright` junto a `github` en `.vscode/mcp.json`:

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--headless"]
    }
  }
}
```

Guarda el archivo. A diferencia del servidor remoto `github`, `playwright` se ejecuta localmente. La opción `--headless` inicia el navegador sin ventana visible, algo necesario en Codespaces.

> [!NOTE]
> El proyecto ya utiliza Playwright para pruebas de extremo a extremo, así que normalmente el navegador está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium`.

## Iniciar y confiar en el servidor

VS Code inicia los servidores MCP cuando hacen falta y pide que confirmes si confías en ellos.

1. En `.vscode/mcp.json`, selecciona **Start** sobre la entrada `playwright` para iniciar el servidor y confirmar la configuración.
2. Si VS Code pregunta si confías en el servidor, revisa la configuración y confirma la confianza.

También puedes omitir este paso; VS Code iniciará el servidor y solicitará confianza cuando Copilot necesite sus herramientas.

## Probar el filtrado

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que use Playwright MCP:

```text
Usa el servidor MCP de Playwright para abrir un navegador en la aplicación en ejecución en http://localhost:4321 y comprobar el nuevo filtrado de juegos:

1. Ve a la página de juegos y anota cuántos aparecen.
2. Aplica un filtro de categoría y confirma que solo aparecen juegos de esa categoría.
3. Quita el filtro y aplica uno de editorial; confirma que la lista se actualiza.
4. Combina los filtros de categoría y editorial y confirma que los resultados cumplen ambos.

Indica lo que observas en cada paso y señala cualquier comportamiento inesperado.
```

Copilot abrirá un navegador mediante Playwright MCP y comprobará cada paso. Revisa el informe según los criterios de aceptación de la incidencia y pide aclaraciones o correcciones si es necesario.

> [!NOTE]
> La aplicación debe estar disponible en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo. La primera vez, Playwright MCP quizá tenga que descargar un navegador.

El servidor MCP de Playwright trabaja con el árbol de accesibilidad de la página, no con capturas de pantalla. El agente usa elementos etiquetados, como botones, enlaces y elementos de lista; por eso una comprobación funcional también sirve como verificación básica de accesibilidad.

## Publicar la rama y crear una PR

1. Abre **Source Control** en el Codespace y revisa los cambios de Copilot.
2. Prepara los cambios con el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle** y selecciona **Publish** para enviar la rama.
4. Abre Copilot Chat, inicia **New Chat** y confirma que **Agent** está seleccionado.
5. Pide a Copilot que encuentre la incidencia de filtrado y cree una PR para la rama actual asociada a esa incidencia.
6. Cuando sea necesario, selecciona **Continue** para autorizar las operaciones de GitHub. Abre el enlace de la PR y revísala, pero no la combines todavía.

## Resumen y próximos pasos

Has aprendido qué es MCP, añadido Playwright MCP, probado el filtrado en un navegador, publicado la rama y abierto una PR. A continuación, [crearás un agente personalizado][next-lesson] para tareas especializadas como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
