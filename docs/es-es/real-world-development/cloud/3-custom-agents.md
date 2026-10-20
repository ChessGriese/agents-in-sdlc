---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados para tareas o ámbitos concretos. Se definen en archivos Markdown de `.github/agents` en el repositorio, o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, `name` y `description`, seguido de un prompt que describe el comportamiento, los conocimientos y las instrucciones del agente.

Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de [WCAG][wcag], uno de seguridad o uno de pruebas.

### Agentes personalizados y habilidades de agente

Tanto los agentes personalizados como las [habilidades de agente][agent-skills-concept] suelen definirse en Markdown y dan instrucciones a la IA. Una forma sencilla de diferenciarlos es que el agente personalizado es quien realiza el trabajo y las habilidades son sus herramientas. El agente tiene su propia ventana de contexto y puede coordinar habilidades e incluso otros agentes.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar el enfoque adecuado para tu entorno.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible, independientemente de la visión o las preferencias de cada persona. Los comentarios indican que algunas personas tienen dificultades para leer el tema oscuro por el bajo contraste. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar o desactivar.

En este ejercicio revisarás un agente de accesibilidad existente y lo utilizarás en Copilot Chat para implementar el modo de alto contraste.

## Revisar el agente de accesibilidad

Vuelve al Codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio en los agentes personalizados.

3. Revisa las responsabilidades, las prácticas recomendadas, los ejemplos de HTML/CSS/JavaScript y los errores frecuentes.

## Crear y asignar una incidencia

1. Abre el repositorio y selecciona **Issues**, **New issue** y **Blank issue**.
2. Asigna el título `Add high contrast mode to website`.
3. Usa esta descripción:

   ```plaintext
   Necesitamos un modo de alto contraste para el sitio. Debe haber un control para activarlo y desactivarlo y la preferencia debe guardarse en el almacenamiento local del navegador.
   ```

4. Selecciona **Create**, luego **Assign to Copilot** y el agente **Accessibility agent**.
5. Selecciona **Assign**.

   ![Asignación del agente en la nube con el agente personalizado de accesibilidad seleccionado](../../../_images/ex5-select-custom-agent.png)

Copilot empezará a trabajar en segundo plano.

## Resumen y próximos pasos

Has explorado los agentes personalizados y has asignado la implementación del modo de alto contraste al agente de accesibilidad. A continuación, [supervisarás y dirigirás la sesión][next-lesson] desde el panel de agentes.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar agentes personalizados en una organización][org-custom-agents]
- [Preparar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
