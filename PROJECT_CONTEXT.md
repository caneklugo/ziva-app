# Contexto del proyecto: Ziva (prototipo-enactus)

> Documento de contexto para cualquier IA o persona. Léelo en lugar de reanalizar todos los archivos.
> **Análisis realizado:** 2026-11-17 · **Último commit de código antes de la pausa:** `a50728f` · **Rama:** `main`
> El commit de pausa (`docs: pausa del plan principal, cambios en curso`) es posterior a `a50728f` e incluye las Fases 1 y 2. Para ver su hash usa `git log -2 --oneline`.
> **Estado: PAUSADO** (2026-11-18). Antes de seguir, lee la sección 8 «Pendientes» y la última fila de la sección 11.
> Si `git log -1` muestra un commit posterior al de pausa, revisa `git diff <commit-de-pausa>..HEAD` y actualiza este documento.

## 1. Propósito
Ziva es un asistente móvil de accesibilidad auditiva para personas sordas o con hipoacusia. Este repositorio es un **prototipo MVP clickeable** (no hay IA, micrófono ni backend reales) para presentar en la **Competencia Nacional de Emprendimiento Social de Enactus México**.

Tres pilares:
1. **Alertas críticas de seguridad:** sirenas, alarmas, disparos, choques, gritos. Muestran pantalla completa, dirección del sonido y vibración.
2. **Transcripción y detección de nombre:** subtítulos en vivo y resalte cuando alguien dice el nombre del usuario (ejemplo: «Daniela»).
3. **Comunicador visual y SOS:** texto en pantalla grande, frases rápidas y envío de ubicación.

Modelo de negocio (en la landing): Gratis (\$0 MXN, alertas críticas siempre gratis, 2 perfiles), Premium Individual (\$79 MXN/mes) y Campus/Empresa (B2B, convenio institucional). Alineado a ODS 3, 4 y 10.

## 2. Estado del repositorio
- Remoto: `origin` → `https://github.com/caneklugo/ziva-app.git`
- Commits: `8b866f5` (landing y prototipo rediseñado) y `a50728f` (tipografía editorial, marca morada, responsive móvil).
- Sin CI, sin tests, sin `package.json`.
- Archivos de documentación añadidos en la sesión de configuración: `PROJECT_CONTEXT.md`, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `README.md` (reescrito), `.gitignore`.
- No hay `.env.example`: no existen variables de entorno. Crearlo cuando haya backend o APIs.

## 3. Stack
- HTML + CSS + JavaScript vanilla, sin build ni dependencias.
- Fuentes por CDN: Google Fonts (Poppins, Playfair Display; Atkinson Hyperlegible solo en `Main.dc.html`).
- React 18 y `react-dom` locales en `vendor/`, **solo** para `Main.dc.html`.
- APIs del navegador usadas: `navigator.vibrate`, Web Audio (tonos sintéticos).
- Despliegue: GitHub Pages desde `main` / raíz (ver `DEPLOY.md`).

## 4. Estructura de archivos
| Archivo | Líneas aprox. | Descripción |
|---|---|---|
| `index.html` | ~1190 | Landing: navbar (sin etiqueta Enactus por ahora), hero con iframe de `app.html` en marco de teléfono (`#demo`), problema y estadísticas (`#problema`), 3 pilares (`#funciones`), catálogo de sonidos (`#alertas`), precios (`#modelo`), impacto ODS (`#impacto`), footer. Variables CSS en `:root` (tema claro, paleta azul `#325978` / `#0D243B` / `#A7CADF`, alertas en `#730F1A`). |
| `app.html` | ~890 | Prototipo móvil Ziva (Fase 2 completada): 5 pestañas (`Inicio`, `Transcripciones`, `Hablar`, `Aprende`, `Perfil`), tema claro azul, Poppins + Playfair Display, 7 alertas, chat IA simulado y modo Free/Premium interactivo. |
| `Main.dc.html` | 834 | **Prototipo legado «Señal».** Design Component (`<x-dc>`) con clase `Component extends DCLogic`. Azul, Atkinson Hyperlegible. No enlazado. |
| `support.js` | — | Runtime generado de dc (minificado). No editar. |
| `vendor/react.js`, `vendor/react-dom.js` | — | React de producción minificado. |
| `README.md`, `DEPLOY.md` | — | Instrucciones de arranque y despliegue. |

Puntos de entrada: `index.html` (landing) → `app.html` (prototipo). `Main.dc.html` es independiente.

## 5. `app.html` (prototipo vigente)
**Pestañas** (`switchTab`): Inicio (`#tab-inicio`), Transcripciones (`#tab-transcripciones`), Hablar (`#tab-hablar`), Aprende (`#tab-aprende`), Perfil (`#tab-perfil`). Activa marcada en Black cherry (`#730F1A`).

**Estado global** (`state`): `tab`, `isListening`, `isPremium`, `profile`, `activeAlerts`, `history`, `transcriptTab`, `folders`, `recents`, `live`, `audioCtx`.

**`ALERTS` (7 sonidos de sección 10):** `nombre`, `timbre`, `sirena` (Ambulancia/Policía), `grito`, `claxon`, `llanto` (Llanto de bebé), `alarma` (Sismo/Incendio). Cada una define título, subtítulo, dirección, ícono, prioridad, patrón de vibración y tono.

**Lógica Free vs Premium (`togglePremium`):**
- **Free:** 2 perfiles (`Universidad` y `Casa`), máximo 3 alertas en el feed de Inicio, todas las alertas protegidas/activas por seguridad, carpetas bloqueadas (pero con botón `+ Nueva transcripción en vivo` funcional).
- **Premium (Demo):** perfiles ilimitados (`Universidad`, `Casa`, `Trabajo`, `Calle`), historial completo de alertas, carpetas navegables + creación/búsqueda, y switches de `activeAlerts` funcionales en Ajustes.

**Otras vistas:** modal de alerta fullscreen en degradado Dark blue (solo botón `Entendido`), `Hablar` (caja de texto + pizarrón ≥ 48 px + `speechSynthesis` + 6 frases rápidas + `S.O.S.`), y `Aprende` («Dudas y Definiciones» con diccionario local de modismos/términos). Todo simulado en cliente, sin persistencia ni red.

## 6. `Main.dc.html` («Señal», legado)
Primer prototipo exportado de una herramienta de diseño. Marca provisional «Señal», paleta azul y fuente Atkinson Hyperlegible.
- Onboarding de 3 preguntas (comunicación, grado de pérdida, lugar de uso).
- Pestañas: Inicio, Transcribir, Comunicar, Leer, Ajustes.
- Alertas: sirena, nombre, timbre, claxon, alarma, grito.
- Módulo «Aprender a leer»: vocabulario, glosa LSM, español fácil, dactilología y quiz.
- Dispositivos (micrófono externo, foco inteligente, auxiliar auditivo, smartwatch «próximamente»), perfiles con bloqueo Premium.
- Requiere servidor HTTP (no `file://`).

## 7. Decisiones de diseño y resumen de bocetos
- **Marca vigente:** **Ziva**, tema claro. «Señal» es el nombre provisional del prototipo legado.
- **Paleta:** Sky blue (`#A7CADF`), Blue jeans (`#325978`, botón primario y texto secundario), Dark blue (`#0D243B`, texto principal y fondos destacados), Silver stars (`#B8C3C7`, bordes y separadores), Black cherry (`#730F1A`, alertas críticas, pestaña activa y S.O.S.). Fondo de página `#F3F7FA` y tarjetas `#FFFFFF` / `#E8F0F6`.
- **Tipografía:** Playfair Display (títulos principales), Poppins (SemiBold para logotipo, botones y menús; Medium para subtítulos; Regular para textos y descripciones; Bold para cifras).
- **Principio:** las alertas de seguridad siempre son gratuitas.
- **Resumen visual de `docs/bocetos/`** (carpeta local excluida de Git):
  - `vision-app-movil.png`: mockup en tema claro con 5 pantallas (Inicio, Transcripciones, Alerta fullscreen en degradado Dark blue, Dudas y Definiciones tipo chat, Perfil con Free y Ziva Premium) + lema «Escucha · Transcribe · Aprende».
  - `logo-1-provisional.jpg`: oreja (Sky blue) con signo de interrogación y burbuja de chat de 3 puntos (Blue jeans) y 3 rayas de sonido (Black cherry) + «Ziva». En `index.html` se usa una aproximación SVG transparente hasta tener el original.
  - `wireframe-pestaña-inicio.jpg` y `wireframe-pestaña-hablar.jpg`: definen barra inferior de **5 pestañas** (Inicio, Transcripciones, Hablar, Aprende, Perfil). «Hablar» incluye caja «Escribe tu mensaje» (Mostrar/Borrar), 6 frases rápidas y botón S.O.S.
- **Decisiones cerradas para Fase 2:** Free incluye 2 perfiles (Casa y Universidad) y últimas 3 alertas; Premium incluye perfiles ilimitados, historial completo y carpetas de transcripción (con botón visible para iniciar nueva transcripción en Free); se elimina «Ver en el mapa» de la pantalla de alerta.

## 8. Problemas conocidos y pendientes
| Estado | Asunto |
|---|---|
| Resuelto | CSS duplicado y roto en `app.html` (~líneas 156-160, regla `.profile-pill`). |
| Resuelto | Identidad visual azul y tipografías Poppins + Playfair Display aplicadas en `index.html` y variables de `app.html` (Fase 1). |
| Resuelto | **Fase 2:** `app.html` rediseñado según `docs/bocetos/` y decisiones cerradas (5 pestañas, 7 alertas, Hablar, Dudas con IA simulada, Perfil Free/Premium real). |
| Resuelto | `user-scalable=no` eliminado de `app.html`; switches de `activeAlerts` y perfiles ahora cambian el comportamiento real en Premium. |
| Pendiente | **Decisiones abiertas del plan de la Fase 3** (el usuario aún no las responde ni aprueba el plan): (1) ¿tiene Android Studio y un teléfono Android físico? (no se encontró Android Studio ni SDK en las rutas estándar; sí hay Java); (2) ¿monorepo web + Android en un solo repo? (recomendado: sí); (3) `applicationId` de la app (propuesto `mx.ziva.app`, no se puede cambiar tras publicar); (4) orden: commit de respaldo → reorganizar → proyecto Android; (5) renombrar o no la carpeta local `prototipo-enactus` (recomendado: no por ahora); (6) hospedaje «por definir» hasta que el usuario traiga su investigación; (7) alcance de la Fase 3: solo andamiaje con alertas simuladas, sin audio real. |
| Pendiente | **Aprobación del plan** `plan_fase3.md` (vive en el directorio de artefactos de la conversación de Antigravity, fuera del repositorio). Resumen: estructura propuesta `web/` (`index.html`, `app.html`), `legacy/` (`Main.dc.html`, `support.js`, `vendor/`), `docs/` (`DEPLOY.md`, `arquitectura-android.md`, `bocetos/` local) y `android/`; mover con `git mv` conservando rutas relativas; Android con Kotlin + Jetpack Compose, MVVM, `minSdk` 26, 5 pestañas, tema Ziva, reglas Free/Premium, `SoundClassifier` con implementación simulada y permisos declarados pero sin pedir. Si el plan no está disponible en el nuevo chat, volver a generarlo con `/plan` a partir de este resumen. |
| Pendiente | **Hospedaje:** GitHub Pages con repo privado exige plan de pago; el usuario investigará alternativas (Cloudflare Pages, Netlify, Vercel, Firebase Hosting) en la app de Gemini y traerá los resultados. Hasta entonces `DEPLOY.md` queda tal cual (con pasos obsoletos). Nota: GitHub Pages «desde rama» solo sirve la raíz o `/docs`, no `web/`. |
| Pendiente | **Organización de carpetas y archivos:** ordenar la estructura del repositorio (separar/agrupar archivos web, legados y documentación manteniendo rutas relativas válidas para GitHub Pages) antes o al iniciar la Fase 3. |
| Pendiente | **Fase 3:** definir arquitectura y base de la app móvil Android (evaluar Kotlin + Jetpack Compose), considerando la nueva estructura organizada del proyecto. |
| Pendiente | **Revisión visual manual** en navegador (`python -m http.server 8000`) de `index.html` y `app.html` tras las Fases 1 y 2. Puntos clave: el prototipo dentro del iframe (la barra de 5 pestañas y los modales deben caber en 780 px escritorio / 600 px móvil), `app.html` en un celular real (vibración, zoom, botones ≥ 44 px, voz) y los flujos Free/Premium. **Nunca se ha visto a detalle el prototipo dentro de la landing.** |
| Pendiente | Reemplazar el SVG aproximado del logo por el archivo original con fondo transparente cuando el usuario lo suba. |
| Pendiente | Agregar en `index.html` una sección futura del camino de desarrollo del proyecto y logros (incluyendo Enactus). |
| Pendiente | Alinear textos de `index.html` con la sección 10 (landing dice «9 situaciones críticas» y lista 8 sonidos; faltan llanto de bebé y timbre, sobran disparos/explosiones). |
| Pendiente | La landing menciona tarjeta de identidad; evaluar si se incluye en la app o se ajusta el texto de `index.html`. |
| Pendiente | Las estadísticas de la landing (+2.3M, 82 %, 100 %) no tienen fuente en el repo. |
| Pendiente | El enlace «GitHub» del footer apunta a `https://github.com` genérico. |
| Pendiente | `DEPLOY.md` incluye pasos que ya no aplican (el remoto ya existe). |
| Pendiente | Revisar la codificación de `README.md` y `DEPLOY.md` si se ven caracteres raros en la terminal. |

## 9. Glosario
- **LSM:** Lengua de Señas Mexicana.
- **Hipoacusia:** pérdida auditiva parcial.
- **ODS:** Objetivos de Desarrollo Sostenible de la ONU.
- **Freemium:** plan gratuito con funciones de pago.
- **Enactus:** red de estudiantes que desarrollan proyectos de emprendimiento social.

## 10. Contexto adicional del equipo
IMPORTANTE: El proyecto se divide en 2 (por el momento, puede cambiar el enfoque más adelante), con la landing page, basicamente todo lo que estamos trabajando hasta ahora, y por otro lado y lo más importante, con lo que trabajaremos más tiempo (supongo), la app móvil primero para android como un sdk (aun no estoy seguro ni lo he planeado bien).
Ahora mismo quiero cambiar colores y fuentes en base a algunos bocetos en imagenes que tengo, por lo que sería util tener donde subir dichas imagenes para que las repliques en el prototipo de la app movil. Sin embargo primero trabajemos con lo que ya existe, que es la landing page. Asi que primero cambiamos colores y fuentes de la landing page, en base a las imagenes de como visualizo la app, y despues empezamos a planear el desarrollo de la aplicación. A continuación la información que hasta ahora tengo aterrizada.

Segmentación del mercado: personas sordas y con poca audición.

Funciones planeadas (para estas funciones también podemos dejarlas por etapas, las que se pueden implementar sin mucha complicación en la primera versión, y despues las que necesitan investigación y un desarrollo más extenso. También las fases pueden ser primero con funciones gratis y luego las preemium, es solo un ejemplo):
Detectar cualquier alarma:
    Que digan su nombre
    Timbre
    Ambulancia/ Policia
    Grito
    Claxon de vehículos
    Llanto de bebe
    Alarma de emergencia tipo de terremoto, incendio
Transcripcion de conversacion en tiempo real:
    Para lo premium que se guarde el historial y puedan guardar cada conversación en carpetas dependiendo el tipo de conservación y hasta por clase.
Últimos eventos escuchados (premium).
    Selección de qué alertas quieren escuchar esto es premium y para no afectar que en la versión normal tengan todas las alertas activadas.
Perfiles por el lugar en el que se esté (premium).
    Parte de voz que es lo de botones para comunicarse básicos y poner la cajita para que ellos escriban lo que sea y se diga: 
        Botón de s.o.s
        Hola, soy sordo
        Gracias
        ¿Puedes decirlo más lento?
        ¿Puedes escribirlo?
        No te entendi
        Necesito ayuda
Dudas/ Definiciones es decir un área de aprendizaje con ia, como si fuera un chat, para cosas que no entiendan y les expliquen para sordos como lo de camarón que se duerme se lo lleva la corriente.

Fuentes: 
Logotipo Ziva: Poppins SemiBold
Titulos principales: Playfair Display
Subtitulos: Poppins Medium
Textos y descripciones: Poppins Regular
Botones y menús: Poppins SemiBold

Colores:
Sky blue #A7CADF
Blue jeans #325978
Dark blue #0D243B
Silver stars #B8C3C7
Black cherry #730F1A

## 11. Historial de sesiones
| Fecha | Qué se hizo |
|---|---|
| 2026-11-17 | Análisis completo del repositorio. Se crearon `PROJECT_CONTEXT.md`, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md` y `.gitignore`. Se reescribió `README.md`. Se corrigió el CSS roto de `app.html`. Sin commit. |
| 2026-11-17 | Fase 1 ejecutada: se ignoró `docs/bocetos/` en `.gitignore`, se aplicó la paleta azul clara (`#A7CADF`, `#325978`, `#0D243B`, `#B8C3C7`, `#730F1A`) y fuentes Poppins + Playfair Display en `index.html`, se reemplazó el logo por una aproximación SVG transparente, se retiró la etiqueta Enactus de la navbar, se actualizaron las variables `:root` de `app.html`, y se actualizaron `AGENTS.md` y `PROJECT_CONTEXT.md`. Sin commit. |
| 2026-11-17 | Fase 2 ejecutada: rediseño completo de `app.html` con 5 pestañas (`Inicio`, `Transcripciones`, `Hablar`, `Aprende`, `Perfil`), 7 alertas de sección 10, modal de alerta en degradado Dark blue sin botón de mapa, lógica real de Free vs Premium y mejoras de accesibilidad (sin `user-scalable=no`, objetivos ≥ 44 px). Actualizado `PROJECT_CONTEXT.md`. Sin commit. |
| 2026-11-18 | **Pausa.** Se revisó `git status`, se redactó el plan de organización del repositorio y de la Fase 3 (Android) y se entregó al usuario un prompt para investigar hospedaje con repo privado. No se modificó código en esta sesión. Se hizo el commit de pausa `docs: pausa del plan principal, cambios en curso` con todo lo de las Fases 1 y 2 (`README.md`, `app.html`, `index.html`, `.gitignore`, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `PROJECT_CONTEXT.md`). |

**Qué quedó incompleto al pausar:**
- **Ningún archivo de código quedó a medias.** `index.html` y `app.html` están en estado funcional (sintaxis JS validada con `node --check`), pero **no se han revisado visualmente en navegador**.
- **El plan de la Fase 3 no está aprobado.** Está escrito pero espera que el usuario lo lea, responda las 7 preguntas abiertas (ver sección 8) y lo apruebe. No se ha movido ni creado ninguna carpeta (`web/`, `legacy/`, `android/`).
- **No se ha elegido hospedaje.** El repositorio sigue público y el sitio asume GitHub Pages.
- `DEPLOY.md`, `README.md` (comando de arranque) y `AGENTS.md` siguen asumiendo que `index.html` está en la raíz; cambiarán cuando se reorganice.
- El logo en `index.html` y `app.html` es una aproximación SVG; falta el original con fondo transparente.
