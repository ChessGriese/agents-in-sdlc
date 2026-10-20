---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Comprueba la configuración de Playwright MCP en Copilot CLI y úsala para probar el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Programar no consiste solo en escribir código: también hay que trabajar con datos, servicios externos y automatizaciones adicionales. Los servidores MCP amplían Copilot CLI con herramientas y servicios que no incluye de forma integrada.

En esta lección aprenderás qué es Model Context Protocol (MCP), añadirás el servidor MCP de Playwright si aún no está disponible y pedirás al agente que controle un navegador para probar la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz también hay que interactuar con ella. Quieres que Copilot pueda usar el sitio web como lo haría una persona y automatizar así la comprobación de los cambios.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite a los agentes de IA comunicarse en tiempo real con herramientas y servicios externos, acceder a información actualizada y realizar acciones en tu nombre. Los servidores MCP actúan como puente entre el agente y las herramientas disponibles; cada servidor ofrece un conjunto distinto de herramientas y recursos.

Entre los servidores MCP conocidos están [GitHub MCP Server][github-mcp], que proporciona acceso a API para administrar repositorios, incidencias y PR de GitHub, y [Playwright MCP Server][playwright-mcp-server], que permite automatizar navegadores con Playwright. GitHub mantiene un [registro MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia: revisa su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad antes de utilizarlo.

## Añadir el servidor MCP de Playwright

Si el servidor de Playwright no está disponible, añádelo para que Copilot pueda interactuar con el sitio web.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos.

1. Vuelve al Codespace.
2. En Copilot CLI, abre el diálogo para añadir un servidor con:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. En el diálogo **Command**, pega:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor.
7. Pulsa <kbd>Esc</kbd> para cerrar el diálogo MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Pide a Copilot que valide el comportamiento mediante Playwright MCP:

```plaintext
Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y nuestro plan. Dime qué funciona y qué no, sin hacer cambios. Detén el servidor que inicies cuando termines.
```

> [!NOTE]
> No es imprescindible indicar que use el servidor MCP: Copilot suele identificarlo automáticamente. Nombrarlo puede ayudarle a elegir la herramienta adecuada.

Observa los pasos que Copilot realiza en el navegador y revisa el informe para confirmar que todo funciona como se espera. Copilot iniciará el servidor, probará el sitio, lo detendrá y resumirá los resultados.

## Resumen y próximos pasos

Has utilizado Playwright MCP para probar la funcionalidad en un navegador real desde Copilot CLI. A continuación, [crearás un agente personalizado de control de calidad][next-lesson] que reúna las habilidades y las herramientas de navegador.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
