---
title: "Lección 5 - Personalizar y usar una aptitud de comprobación de calidad"
description: "Explora la aptitud existente quality-checks, personaliza el formato de su informe y úsala para verificar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has podido validar manualmente que el código funciona y has usado archivos de instrucciones para asegurar que sigue los estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Las **aptitudes de agente** son adecuadas para este tipo de tareas. Ayudan a Copilot a entender cómo ejecutar operaciones como estas correctamente.

En esta lección:

- explorarás la aptitud existente `quality-checks`.
- personalizarás el formato de los resultados.
- volverás a cargar la aptitud y la ejecutarás.

## Escenario

Tailspin Toys usa la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las aptitudes de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, cada aptitud es una carpeta con su nombre y un archivo Markdown `SKILL.md`. El archivo contiene metadatos frontmatter con el nombre y la descripción, una descripción general de la aptitud e indicaciones sobre cuándo debe usarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las subcarpetas y los archivos adicionales son opcionales. La aptitud `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las aptitudes pueden estar en la carpeta `.github/skills` del proyecto para que el equipo las comparta y reutilice, o en la carpeta de aptitudes del usuario `~/.copilot/skills`.

## Explora la aptitud

Explora la aptitud que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe usar la aptitud.
3. Lee las instrucciones y fíjate en cómo guían a Copilot durante las pruebas y el lint.
4. Observa que la aptitud todavía no contiene una sección **Results output formatting**.

## Ejecuta la aptitud antes de hacer cambios

Las aptitudes se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la aptitud.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía esta solicitud:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe que aparece al final.

## Personaliza el informe

Queremos que el informe indique qué se ha ejecutado, si se ha superado y qué han comunicado realmente las herramientas. Actualiza la aptitud para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la aptitud actualizada

Veamos cómo funciona el cambio. Copilot CLI puede volver a cargar las aptitudes modificadas sin que tengas que reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía la misma solicitud que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una aptitud de agente existente. En esta lección:

- has explorado la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de los resultados.
- has vuelto a cargar y ejecutado la aptitud.

Ese cambio acompañará a los filtros en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos de la comunidad sirven de referencia y no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
