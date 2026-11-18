# Ziva · Asistente de accesibilidad auditiva

Prototipo MVP de alta fidelidad para la competencia de Enactus México. Ziva ayuda a personas sordas o con hipoacusia con alertas de seguridad (visuales y hápticas), transcripción con detección del nombre propio y un comunicador visual con botón SOS.

Es un sitio **estático**: no necesita instalar dependencias ni compilar. Todo el comportamiento es simulado.

## Cómo arrancar

Requiere cualquier servidor HTTP local. Con Python:

```bash
python -m http.server 8000
```

Después abre:

- Landing: http://localhost:8000/
- Prototipo móvil a pantalla completa: http://localhost:8000/app.html
- Prototipo legado «Señal»: http://localhost:8000/Main.dc.html

Alternativa con Node: `npx serve .`

> Algunos navegadores bloquean scripts con `file://`, por eso se recomienda usar servidor local. La tipografía se carga desde Google Fonts y Fontshare, así que necesitas internet para verla bien.

## Estructura

| Archivo | Descripción |
|---|---|
| `index.html` | Landing con el prototipo incrustado en un iframe |
| `app.html` | Prototipo móvil «Ziva» (HTML, CSS y JS en un solo archivo) |
| `Main.dc.html` | Prototipo legado «Señal» (no enlazado) |
| `support.js`, `vendor/` | Runtime y React para `Main.dc.html` (no editar) |
| `DEPLOY.md` | Guía de despliegue en GitHub Pages |
| `PROJECT_CONTEXT.md` | Contexto completo del proyecto |
| `AGENTS.md` | Reglas para agentes de IA (`CLAUDE.md` y `GEMINI.md` apuntan a él) |

## Despliegue

Se publica en GitHub Pages desde la rama `main`, carpeta raíz. Ver [DEPLOY.md](DEPLOY.md).

## Trabajar con IA

Cualquier modelo debe leer primero [AGENTS.md](AGENTS.md) y [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md).
