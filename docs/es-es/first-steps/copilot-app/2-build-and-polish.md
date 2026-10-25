---
title: "Lección 2 - Crear y perfeccionar el cuestionario"
description: "Crea un Space Quiz en un solo archivo y perfecciónalo con el navegador integrado y el selector de elementos."
authors:
  - jamesmontemagno
lastUpdated: 2026-09-28
---

Utiliza un único prompt detallado para crear el Space Quiz, comprueba su comportamiento en el navegador integrado y realiza un ajuste visual con el selector de elementos.

En esta lección:

- crearás un cuestionario sin dependencias en `index.html`.
- probarás el cuestionario en el navegador integrado.
- inspeccionarás el código generado.
- perfeccionarás un elemento seleccionado sin perder la accesibilidad.

## Crea el cuestionario

Envía el siguiente prompt en tu sesión `space-quiz`:

```plaintext
Crea un cuestionario de exploración espacial con 10 preguntas, una barra de progreso, un contador de puntuación y respuestas visuales animadas y coloridas (verde para las respuestas correctas y una sacudida roja para las incorrectas). Al final, muestra una pantalla de resultados con una reacción con un emoji. Centra el contenido en una columna estrecha. Usa un único archivo index.html, sin servidor ni dependencias. Dale un acabado cuidado, con una fuente sans serif y texto de 14–16 px, y respeta prefers-color-scheme. Ábrelo en el navegador integrado.
```

Cuando el agente termine, responde a varias preguntas y confirma que:

- la barra de progreso avanza.
- la puntuación se actualiza.
- las respuestas correctas muestran un estado verde.
- las respuestas incorrectas utilizan una animación de sacudida roja.
- la pantalla de resultados aparece después de la última pregunta.

## Inspecciona el código generado

Contrae la barra lateral izquierda y el panel del explorador de la derecha y, después, revisa `index.html` en el área de código ampliada. Observa cómo el lenguaje de marcado de hipertexto (HTML), las hojas de estilo en cascada (CSS) y JavaScript funcionan juntos en un único archivo. Restaura ambos paneles cuando termines.

## Perfecciona con el selector de elementos

1. Selecciona el selector de elementos en la barra de herramientas del explorador.
2. Selecciona el encabezado del cuestionario o el área de respuestas.
3. Envía el siguiente prompt:

   ```plaintext
   Haz que el elemento seleccionado se parezca más a una pantalla de control de misión. Mantén la accesibilidad y conserva los temas claro y oscuro existentes.
   ```

4. Observa cómo se actualiza el navegador integrado y comprueba el cambio.

## Ajustes opcionales

Si quieres seguir experimentando, pide al agente que:

- añada un fondo sutil de estrellas que respete `prefers-reduced-motion`.
- haga que la pantalla de resultados sea más festiva cuando la puntuación sea de 8 o más.
- mejore los estados de foco del teclado y, después, comprueba el cuestionario sin ratón.

## Resumen y pasos siguientes

Has creado, inspeccionado y perfeccionado el Space Quiz. Continúa con la [lección 3: Inspeccionar la sesión y probar el cuestionario][next-lesson].

[next-lesson]: ../3-inspect-and-test/
