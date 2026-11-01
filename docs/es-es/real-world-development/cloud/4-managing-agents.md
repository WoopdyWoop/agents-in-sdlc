---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente de GitHub Copilot en la nube que se encargara de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas del agente que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones que se han realizado y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de alternancia, parecía lógico incluir esta funcionalidad. Guiarás el trabajo del agente para que añada tanto el modo claro como el de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel de la izquierda. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - Actualizar la documentación del código.
    - Generar API para modificar juegos.
    - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa el trabajo que ha realizado Copilot. Entre otras cosas, podría:
    - obtener el código del repositorio.
    - crear el entorno de trabajo de Copilot.
    - configurar servidores MCP.
    - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos que se muestren dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios, que aparece a la derecha y permite ver la PR y los archivos modificados para seguir el trabajo.

## Guía al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada un control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya asignado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, introduce esta solicitud:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes con el cuadro de diálogo para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la solicitud.
5. Observa cómo Copilot la confirma y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando al recibir el mensaje.

El trabajo tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente en la nube. Puedes seguir todas las tareas activas y completadas, revisar el trabajo realizado y redirigir tareas en curso para ajustar el alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- explorar Mission Control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las mejoras de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
