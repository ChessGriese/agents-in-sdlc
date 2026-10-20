---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados para tareas o ámbitos concretos. Se definen en archivos Markdown de `.github/agents` e incluyen frontmatter YAML con, al menos, `name` y `description`, seguido de las instrucciones para el agente. También pueden definirse globalmente en `~/.copilot/agents`.

Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de [WCAG][wcag], uno de seguridad o uno de pruebas.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente con Markdown, pero cumplen funciones distintas: el agente personalizado realiza el trabajo y las habilidades son herramientas. Un agente puede coordinar habilidades y otros agentes, y tiene su propia ventana de contexto.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar lo que mejor funcione en tu entorno.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible independientemente de la visión o las preferencias de cada persona. Los comentarios indican que algunas personas tienen dificultades para leer el tema oscuro por el bajo contraste. El equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar.

En este ejercicio revisarás un agente de accesibilidad existente y lo usarás en Copilot Chat para implementar el modo de alto contraste.

## Revisar el agente de accesibilidad

Vuelve al Codespace y crea una rama nueva desde `main` para mantener separado este trabajo de la PR de filtrado del ejercicio anterior:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Revisa el frontmatter YAML y sus campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios en los agentes personalizados.

3. Revisa las responsabilidades, las prácticas recomendadas, los ejemplos de HTML/CSS/JavaScript y los errores frecuentes.

## Usar el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes los perfiles definidos en `.github/agents`. Al seleccionar uno, la conversación utiliza sus instrucciones y herramientas.

1. Abre Copilot Chat, selecciona **New Chat** y confirma que **Agent** está seleccionado.

   ![Selector de agentes de la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. En el selector inferior, selecciona **Accessibility agent**.
3. Envía este prompt:

   ```plaintext
   Añade un modo de alto contraste al sitio. Debe haber un control para activarlo y desactivarlo y la preferencia debe conservarse al recargar la página mediante el almacenamiento local del navegador.
   ```

4. Revisa los cambios propuestos y aplicados a los archivos del proyecto. El indicador **Files changed** muestra el conjunto de trabajo que el agente ha modificado.

El proceso puede tardar varios minutos mientras Copilot modifica componentes Astro, CSS y pruebas. Revisarás y dirigirás este trabajo en la siguiente lección.

## Resumen y próximos pasos

Has revisado cómo se definen los agentes personalizados y has utilizado el agente de accesibilidad en modo agente de Copilot Chat. A continuación, [supervisarás y dirigirás el trabajo en curso][next-lesson] y pedirás que la misma sesión añada un modo claro.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
