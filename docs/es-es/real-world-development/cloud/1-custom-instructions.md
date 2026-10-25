---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando trabajas con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información previa, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo qué debe hacer, sino también cómo.

En este ejercicio, harás lo siguiente:

- Explorarás cómo llegan al agente las directrices de programación y documentación y el contexto específico del proyecto mediante instrucciones del repositorio e instrucciones asociadas a rutas.
- Añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no compararás un prompt antes y después: el agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil observar el cambio en tiempo real. Verás la influencia de las instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere.

## Archivos de instrucciones

### Escenario

Tailspin Toys sigue directrices y requisitos de desarrollo: la capa de datos debe tener pruebas unitarias; la interfaz debe usar un tema oscuro moderno; el código debe incluir comentarios de TypeScript Documentation (TSDoc); y cada archivo debe empezar con un bloque de comentarios que describa su propósito. Los archivos de instrucciones proporcionan a Copilot esta información.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar a Copilot contexto y preferencias sobre el estilo de programación, las bibliotecas y los comentarios. Puedes crear instrucciones para todo el repositorio o para determinados tipos de archivos y tareas.

Hay dos tipos:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot en todas las solicitudes del repositorio. Debe incluir contexto general del proyecto, como la pila tecnológica, las prácticas recomendadas y las directrices comunes.
- `.github/instructions/*.instructions.md`, archivos con instrucciones para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat; no se aplican a las sugerencias de finalización ni a las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan las instrucciones del repositorio y los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también admite otros estándares de instrucciones, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar instrucciones

Los archivos de ejemplo del proyecto muestran un enfoque posible:

- Mantén `copilot-instructions.md` centrado en las directrices generales del proyecto.
- Usa `*.instructions.md` para tipos de archivo o tareas concretas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de código correcto e incorrecto.

No hay una única forma de crear instrucciones. La experimentación permite averiguar qué funciona mejor en cada proyecto.

> [!TIP]
> Los proyectos que usan GitHub Copilot pueden incluir una colección útil de instrucciones. Revisa las instrucciones para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions]. Copilot también puede ayudarte a generarlas; la forma de hacerlo depende del entorno. Para encontrar plantillas, consulta [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Revisa los archivos de instrucciones que incluye el repositorio. Puedes abrirlos en la interfaz web de GitHub o en un Codespace.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas para los flujos de trabajo de GitHub Actions.
3. Abre la carpeta `.github/instructions` y revisa las instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`. Define un patrón relativo a la raíz del repositorio, por ejemplo, `**/*.test.ts` para archivos de pruebas TypeScript.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación, pero aún no exige documentación en el código. Añadirás esa regla en los pasos siguientes.

## Añade un estándar general del repositorio

Añadirás una regla general para que la documentación del código incluya comentarios TSDoc y un encabezado de comentarios. El agente en la nube la usará cuando trabaje en incidencias posteriores. Como el agente leerá las instrucciones de la rama a la que se dirige la incidencia, combina primero el cambio en `main`.

1. Desde el terminal del Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abre `.github/copilot-instructions.md`.
3. Busca la sección **Code formatting requirements**, aproximadamente en la línea 27.
4. Debajo de los estándares existentes, añade:

   ```markdown
   - Cada función exportada debe incluir un comentario TSDoc que describa su propósito, sus parámetros y su valor devuelto.
   - Antes de las importaciones o de cualquier código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones del repositorio son adecuadas para los estándares generales del proyecto; usa archivos `*.instructions.md` para lenguajes, tipos de archivo y tareas concretos.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. Por eso, las nuevas instrucciones deben llegar a `main` para que las use en el siguiente ejercicio.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar el cambio en `main`. La rama sirve para practicar un flujo habitual de trabajo.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Observarás su efecto en las solicitudes de incorporación de cambios que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
