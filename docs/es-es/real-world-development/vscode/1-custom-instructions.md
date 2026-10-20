---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental para la IA generativa. Los [archivos de instrucciones][instruction-files] explican cómo generar el código, no solo qué código crear. En este ejercicio explorarás las instrucciones del repositorio, generarás una primera función auxiliar de datos con las directrices actuales, añadirás un estándar general y volverás a ejecutar el mismo prompt para observar su efecto.

En concreto, explorarás las instrucciones personalizadas y las instrucciones específicas de rutas; crearás la primera parte de los datos para el filtrado (una función auxiliar de editoriales); añadirás una norma para todo el repositorio en `.github/copilot-instructions.md`; compararás los resultados y confirmarás los cambios en `main` para que estén disponibles en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir siempre los estándares indicados. Copilot no es determinista; el objetivo es observar la tendencia después de actualizar las instrucciones, no obtener una salida idéntica.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene varias directrices: la capa de datos necesita pruebas unitarias; la interfaz debe tener un tema oscuro y moderno; el código debe documentarse con comentarios TSDoc; y cada archivo debe comenzar con un bloque que explique su propósito. Las instrucciones aportarán ese contexto a Copilot.

### Instrucciones personalizadas

- `.github/copilot-instructions.md` se envía a Copilot en todas las solicitudes del repositorio y contiene contexto general del proyecto.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> En el IDE, las instrucciones solo se usan para generar código en Copilot Chat; no se aplican a las completaciones ni a las sugerencias de siguiente edición. Copilot Chat, CLI y el agente en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`. También se admiten formatos como AGENTS.md y CLAUDE.md.

## Explorar los archivos de instrucciones

1. En el Codespace, abre `.github/copilot-instructions.md` y revisa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**.
2. Explora `.github/instructions`; hay directrices para Astro, la capa de datos Drizzle, pruebas y otras tareas.
3. Abre `.github/instructions/unit-tests.instructions.md` y revisa `applyTo`, que determina los archivos a los que se aplican estas instrucciones.
4. Abre `.github/instructions/drizzle.instructions.md` y consulta los enlaces a otros archivos y ejemplos. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` aún no exige documentación dentro del código. Añadirás esa norma más adelante.

## Crear una rama

Vuelve al Codespace y abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux). Crea una rama:

```bash
git checkout -b custom-instructions
```

## Probar Copilot Chat antes de cambiar las instrucciones

> [!TIP]
> Abre Copilot Chat, selecciona **New Chat** y empieza una conversación limpia. El modo y el modelo se indican en cada ejercicio cuando sea necesario.

1. Cierra las pestañas del editor que no necesites.
2. Abre `src/lib/publishers.ts` para indicar dónde debe estar la función auxiliar.
3. Selecciona **Agent** en el selector de agentes.
4. Envía este prompt:

   ![Selector de agentes de la vista Chat](../../../_images/shared-chat-mode-selector.png)

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales con su nombre e identificador. Aplica los cambios a los archivos.
   ```

5. Copilot explorará el proyecto y aplicará cambios, quizá en la función y en sus pruebas.
6. Comprueba que la función recibe un cliente `db` como primer argumento y devuelve un array tipado, conforme a `.github/instructions/drizzle.instructions.md`.
7. Observa si el código carece de comentarios TSDoc y de un encabezado de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios incluso antes de cambiar las instrucciones. No pasa nada: el objetivo es observar la mejora de coherencia después de la actualización.

## Añadir un estándar al repositorio

1. Abre `.github/copilot-instructions.md` y localiza **Code formatting requirements**.
2. Añade estas reglas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. Guarda el archivo.

> [!TIP]
> Las instrucciones del repositorio son adecuadas para estándares generales; los archivos `*.instructions.md` sirven para lenguajes, tipos de archivo o tareas concretas.

## Volver a ejecutar el prompt

1. En Copilot Chat, selecciona **New Chat**.
2. Abre de nuevo `src/lib/publishers.ts`.
3. Envía el mismo prompt del paso anterior.
4. Comprueba si el archivo y la función incluyen comentarios TSDoc. Un ejemplo de encabezado podría explicar que el archivo contiene funciones auxiliares para consultar editoriales; el comentario de función describiría su propósito, el parámetro `db` y el valor devuelto.

   Por ejemplo, el comentario de función puede indicar que devuelve una lista de editoriales con su identificador y nombre, que `db` es el cliente Drizzle y que el resultado es una promesa con un array de objetos de editorial.

   ```typescript
   /**
    * Devuelve una lista de editoriales con su identificador y nombre.
    *
    * @param db - El cliente de base de datos Drizzle.
    * @returns Una promesa que se resuelve en un array de objetos de editorial.
    */
   ```

## Confirmar las instrucciones y enviar la rama

1. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba los cambios con `git status`.
3. Prepara los cambios de instrucciones y todos los archivos que Copilot haya modificado para la función auxiliar y sus pruebas. Confirma los cambios con un mensaje descriptivo:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios de apoyo, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Copilot puede encontrar la incidencia relacionada, crear una solicitud de incorporación de cambios (PR) desde la rama y vincularla a la incidencia.

1. Abre Copilot Chat en el Codespace y selecciona **Agent**.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia relacionada y cree una PR desde la rama actual, asociándola con esa incidencia e indicando que también se ha añadido la función auxiliar de editoriales para el trabajo de filtrado:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que cierra esa incidencia e incluye que también hemos añadido la función auxiliar de editoriales para el próximo trabajo de filtrado.
   ```

4. Revisa cada solicitud de permiso de GitHub y selecciona **Allow** cuando corresponda.
5. Cuando se cree la PR, pide a Copilot que la combine en `main`, cambie a `main` en local y obtenga los cambios más recientes:

   ```plaintext
   Combina la PR en main. Después, vuelve localmente a main y obtén el código más reciente para estar al día.
   ```
6. Revisa las acciones de GitHub y de terminal antes de autorizarlas.

## Resumen y próximos pasos

Has comparado la generación de código antes y después de modificar las instrucciones, has añadido un estándar al repositorio y has confirmado las instrucciones y la función auxiliar para que el siguiente ejercicio pueda utilizarlas. A continuación, [usarás el modo agente][next-lesson] para crear una funcionalidad en varias partes del proyecto.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
