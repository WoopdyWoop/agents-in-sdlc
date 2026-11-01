---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Administrar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le pediste que documentara el código, desarrollara una función de juegos relacionados para que el equipo de diseño pudiera iterar sobre ella e implementara mejoras de accesibilidad con controles para los modos de alto contraste y claro. Revisarás los cambios de código y, si hace falta, enviarás comentarios a Copilot para mejorar su trabajo.

### Escenario

Como ya hemos indicado varias veces, añadir IA generativa no cambia los fundamentos del diseño de software ni de DevOps. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisarás las sugerencias de GitHub Copilot para la documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y el agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para garantizar que el entorno siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para los cambios.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno temporal independiente para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se puedan ejecutar.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por explorar la primera solicitud de incorporación de cambios (PR) que generó el agente de Copilot en la nube: añadir documentación al código. Para ello, usarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, es posible que veas una advertencia que indica que el firewall bloquea a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de Copilot en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation** o algo parecido.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios de documentación TSDoc. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas mejoras provienen de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si los flujos de trabajo esperan aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una PR no es una interacción unidireccional. Puedes mencionarlo en comentarios, igual que harías con cualquier otra persona del equipo, tanto en la PR como en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos recomendar un texto exacto. Puedes pedirle, por ejemplo, que:

- añada al principio de cada archivo de código encabezados de comentario con una breve descripción de su propósito.
- añada comentarios de documentación TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia relacionada con la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, verás una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, marca la PR como lista para revisión y combínala en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot ha generado para resolver la incidencia de los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page** o algo parecido.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
7. Si los flujos de trabajo esperan aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve a tu Codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, como **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente de Copilot en la nube y pedirle que haga cambios. Por ejemplo, podrías pedirle que ajuste cuántos juegos relacionados se muestran o que añada encabezados de comentario y comentarios de documentación TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para hacer esas solicitudes, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las mejoras de accesibilidad

Por último, revisa las mejoras de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde Mission Control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website** o algo parecido.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si los flujos de trabajo esperan aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las mejoras de accesibilidad. Ve a tu Codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan como esperas y que la preferencia se conserva después de volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo las prácticas recomendadas. Si detectas problemas o mejoras de accesibilidad, puedes mencionar a **@copilot** en un comentario y pedirle que los resuelva, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando puede trabajar en elementos reales del backlog mientras tú te ocupas de otras tareas. Para crear ese hábito, registra algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para indicar interés en los juegos en la página de detalles.
- Implementar paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los asistentes de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas incluidos en el repositorio** para ver más adelante su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has visto cómo prepara el entorno, planifica y ejecuta las tareas de forma asíncrona.
- **Has creado y usado un agente personalizado** para accesibilidad y has añadido controles para los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad mientras trabajaba.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has aprendido:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, aptitudes de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Administrar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
