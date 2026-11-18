# Reglas compartidas para agentes de IA

Aplican a cualquier modelo o herramienta (Claude, Gemini, GPT, Cursor, etc.) que trabaje en este repositorio.

## Antes de empezar
1. Lee este archivo y luego [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md). **No releas todos los archivos del proyecto**; lee solo los relevantes para la tarea.
2. Compara el commit indicado en `PROJECT_CONTEXT.md` con `git log -1`. Si difiere, revisa `git diff <commit>..HEAD` para ver qué cambió.
3. Revisa `git status` antes de modificar nada.

## Reglas
- **Idioma:** toda la documentación, comentarios y textos de interfaz van en español.
- **Stack:** sitio estático (HTML, CSS y JS vanilla). No introduzcas frameworks, bundlers ni `package.json` sin aprobación del usuario.
- **Rutas relativas:** el sitio se publica en GitHub Pages dentro de un subdirectorio. Nunca uses rutas absolutas (`/app.html`).
- **No editar:** `support.js` (generado) ni `vendor/` (terceros).
- **Legado:** `Main.dc.html` («Señal») es un prototipo legado. No lo modifiques salvo petición expresa.
- **Accesibilidad:** el público son personas sordas o con hipoacusia. Mantén buen contraste, objetivos táctiles de al menos 44 px, no dependas solo del sonido y no bloquees el zoom.
- **Marca vigente:** «Ziva», tema claro, paleta azul (`#A7CADF` Sky blue, `#325978` Blue jeans, `#0D243B` Dark blue, `#B8C3C7` Silver stars, `#730F1A` Black cherry), tipografías Poppins y Playfair Display.
- **Bocetos:** antes de cualquier trabajo visual, revisa `docs/bocetos/` (solo local, ignorado en Git) y el resumen visual en [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md).
- **Secretos:** nunca incluyas claves ni credenciales en el repositorio.
- **Cambios mínimos:** haz solo lo pedido. Si detectas otros problemas, repórtalos sin corregirlos.
- **Git:** no hagas commit ni push sin que el usuario lo pida. Mensajes de commit con prefijo `feat:`, `fix:`, `docs:` o `refactor:`.
- **Comprobación:** para probar, sirve el proyecto con `python -m http.server 8000` y abre `http://localhost:8000/`.

## Al terminar una sesión
- Actualiza "Historial de sesiones" y "Pendientes" en [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md).
- Actualiza el commit de referencia y la fecha si hubo commits.
- Deja claro qué quedó hecho y qué falta, para que el siguiente modelo continúe sin reanalizar.
