---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, Copilot necesita conocer esa orientación. Los [archivos de instrucciones][instruction-files] describen no solo qué código quieres, sino también cómo debe estructurarse. Ahora que has implementado el filtrado, revisarás las instrucciones que utilizó Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las específicas de rutas.
- actualizarás el archivo de instrucciones para que se respeten los estándares de programación.
- observarás cómo afectan las instrucciones al código.

## Escenario

Tailspin Toys tiene directrices para sus prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc/JSDoc, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices existentes de formato y lint.

Los archivos de instrucciones ayudan a que Copilot disponga del contexto necesario para seguir estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias sobre el estilo y los requisitos del código.

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener contexto general del proyecto.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones del proyecto

1. Vuelve al Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Revisa la descripción del proyecto y las directrices de programación, que se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora los archivos de `.github/instructions`, incluidas las instrucciones para Astro, la capa de datos Drizzle y las pruebas.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`, que define los archivos a los que se aplican las instrucciones.
6. Revisa los enlaces de `.github/instructions/drizzle.instructions.md` a otros archivos de instrucciones y ejemplos del proyecto.

## Actualizar las instrucciones según las directrices del equipo

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, aproximadamente en la línea 35.
2. Añade este elemento al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Tras guardar los cambios, sal y reanuda la conversación de filtrado para que Copilot cargue las nuevas directrices sin perder el contexto.

1. Si la conversación de filtrado sigue abierta, ciérrala con `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, reanuda la conversación:

   ```bash
   copilot --resume
   ```

   Si se te pregunta, selecciona la conversación de filtrado y confirma que estás en la rama `game-filters-cli`.
3. Pide a Copilot que aplique las nuevas directrices:

   ```plaintext
   Acabamos de actualizar las instrucciones y las directrices del código. Actualiza el código que has generado para que las siga.
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los nuevos comentarios TSDoc explican correctamente el código.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI utiliza los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. Ahora [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para garantizar que las comprobaciones y las pruebas se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
