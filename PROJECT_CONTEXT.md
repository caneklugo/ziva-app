# Contexto del proyecto: Ziva (prototipo-enactus)

> Documento de contexto para cualquier IA o persona. Léelo en lugar de reanalizar todos los archivos.
> **Última actualización:** 2026-11-20 · **Último commit confirmado al cerrar:** `006950c` (quedan pendientes de commit los cambios del punto 8 en `index.html`, más esta actualización documental) · **Rama:** `main`
> El plan principal (Fase 3) quedó en pausa en `8936c18`. Entre `47d9e91` y `006950c` (y el punto 8) se trabajaron 8 ajustes puntuales de frontend.
> Antes de retomar el plan principal, lee la sección 8 «Pendientes» y la última fila de la sección 11.

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
| `index.html` | 1563 | Landing: navbar compacta (58 px), hero centrado de 2 columnas (iframe de `app.html` a la izquierda y título/botón a la derecha, sin badge superior), problema y estadísticas (`#problema`), 3 pilares (`#funciones`), catálogo de sonidos (`#alertas`), precios (`#modelo`, con botón de Campus & Empresa enlazado al Instagram `@enactustecmicuu`), bloque Compromiso Enactus (ODS 10, 9 y 4 + logo circular de Enactus Tecmilenio Chihuahua con enlace a Instagram) + Tracking Record en línea de tiempo escalonada de 5 hitos con lightbox (`#impacto`), footer. Variables CSS en `:root` (tema claro, paleta azul `#325978` / `#0D243B` / `#A7CADF`, alertas en `#730F1A`). |
| `app.html` | 2347 | Prototipo móvil Ziva: barra inferior de **4** pestañas (`Inicio`, `Transcripciones`, `Hablar`, `Aprende`; `Perfil` se abre desde el icono superior derecho en `Inicio` o desde «Ver Premium»), escucha pausable tocando el círculo radar (cambia a icono de micrófono cruzado), carpetas navegables con reproductor simulado en Premium, modal para renombrar y elegir carpeta al guardar transcripción (Premium), fichas de Recientes con fecha dinámica y degradado, y avisos tipo notificación con colores por sentido (`success`, `info`, `error`). |
| `assets/tracking-record/` | — | 5 fotos originales del recorrido (`1-...` a `5-pej.jpeg`), `hub-chih-logo.png` y subcarpeta `thumbs/` con las 5 miniaturas recortadas en `16:9`. (`descripciones.md` local está ignorado en `.gitignore`). |
| `Main.dc.html` | 834 | **Prototipo legado «Señal».** Design Component (`<x-dc>`) con clase `Component extends DCLogic`. Azul, Atkinson Hyperlegible. No enlazado. |
| `support.js` | — | Runtime generado de dc (minificado). No editar. |
| `vendor/react.js`, `vendor/react-dom.js` | — | React de producción minificado. |
| `README.md`, `DEPLOY.md` | — | Instrucciones de arranque y despliegue. |

Puntos de entrada: `index.html` (landing) → `app.html` (prototipo). `Main.dc.html` es independiente.

## 5. `app.html` (prototipo vigente)
**Pestañas** (`switchTab`): barra inferior de 4 pestañas — Inicio (`#tab-inicio`), Transcripciones (`#tab-transcripciones`), Hablar (`#tab-hablar`), Aprende (`#tab-aprende`). Perfil (`#tab-perfil`) se abre desde el botón superior en Inicio o desde los avisos de Premium. Activa marcada en Black cherry (`#730F1A`).

**Estado global** (`state`): `tab`, `isListening`, `isPremium`, `profile`, `activeAlerts`, `history`, `transcriptTab`, `folders`, `openFolder`, `player`, `transcripts`, `live`, `audioCtx`.

**`ALERTS` (7 sonidos de sección 10):** `nombre`, `timbre`, `sirena` (Ambulancia/Policía), `grito`, `claxon`, `llanto` (Llanto de bebé), `alarma` (Sismo/Incendio). Cada una define título, subtítulo, dirección, ícono, prioridad, patrón de vibración y tono.

**Lógica Free vs Premium (`togglePremium`):**
- **Free:** 2 perfiles (`Universidad` y `Casa`), máximo 3 alertas en el feed de Inicio, todas las alertas protegidas/activas por seguridad, carpetas bloqueadas (pero con botón `+ Nueva transcripción en vivo` funcional que guarda directo en Recientes).
- **Premium (Demo):** perfiles ilimitados (`Universidad`, `Casa`, `Trabajo`, `Calle`), historial completo de alertas, carpetas navegables con vista de transcripciones y reproductor simulado, renombrado y asignación de carpeta al guardar nota en vivo, búsqueda en transcripciones y switches de `activeAlerts` funcionales en Ajustes.

**Otras vistas y avisos:** modal de alerta fullscreen en degradado Dark blue (solo botón `Entendido`), `Hablar` (caja de texto + pizarrón ≥ 48 px + `speechSynthesis` + 6 frases rápidas + `S.O.S.`), `Aprende` («Dudas y Definiciones» con diccionario local de modismos/términos) y `showToast(msg, type)` con tarjetas inferiores de fondo tenue y barra izquierda según el tipo (`success` verde, `info` azul, `error` vino). Todo simulado en cliente, sin persistencia ni red.

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
| Resuelto | **Revisión visual en navegador** de `index.html` y `app.html` (hecha en vivo con `live-server` durante los ajustes de frontend del 18 al 20 de nov). Queda pendiente únicamente probar en un celular físico real (vibración háptica y `speechSynthesis`). |
| Resuelto | Se agregó en `index.html` (`#impacto`) el bloque de Compromiso Enactus (ODS 10, 9 y 4 + logo de Enactus Tecmilenio Chihuahua enlazado a Instagram) y el **Tracking Record** en línea de tiempo de 5 hitos con fotos, miniaturas `16:9` y lightbox. |
| Pendiente | En `app.html`, añadir carpeta fija/vista para transcripciones sin carpeta o botón «Mover a…» (propuesta pendiente de implementar, ya que hoy solo se muestran en `Recientes`). |
| Pendiente | Reemplazar el SVG aproximado del logo por el archivo original con fondo transparente cuando el usuario lo suba. |
| Pendiente | Alinear textos de `index.html` con la sección 10 (landing dice «9 situaciones críticas» y lista 8 sonidos; faltan llanto de bebé y timbre, sobran disparos/explosiones). |
| Pendiente | La landing menciona tarjeta de identidad; evaluar si se incluye en la app o se ajusta el texto de `index.html`. |
| Pendiente | Las estadísticas de la landing (+2.3M, 82 %, 100 %) no tienen fuente en el repo. |
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

| 2026-11-18 a 2026-11-20 | **Ajustes puntuales de frontend (8 puntos):** (1) se quitó Perfil de la barra inferior de `app.html` (`47d9e91`); (2) el círculo radar de Inicio pausa/reanuda la escucha con icono de micrófono cruzado (`38b36c5`); (3) carpetas navegables con reproductor simulado en Premium (`239d4d5`); (4) renombrar y asignar carpeta al guardar nota en vivo en Premium + nuevo diseño de 3 renglones con fecha dinámica y degradado en Recientes (`940ae6d`); (5) avisos `showToast` tipo tarjeta con fondo tenue y colores por sentido (`5ced143`); (6) botón Campus & Empresa en `index.html` enlazado a Instagram `@enactustecmicuu` (`f12e033`); (7) sección Compromiso Enactus + Tracking Record de 5 hitos con `assets/tracking-record/` (`thumbs/` `16:9` + lightbox), logo circular y `.gitignore` actualizado (`006950c`); (8) reacomodo del hero de `index.html` (sin badge superior, navbar de 58 px, 2 columnas centradas con prototipo completo a la izquierda y texto a la derecha, sin texto bajo el teléfono). Etiquetas `base-inicio-ajustes-frontend` y `base-punto-01` a `base-punto-08` creadas. |

**Estado al cerrar (2026-11-20):**
- **Siguen pendientes del plan principal (sin cambios respecto al 2026-11-18):** aprobación de `plan_fase3.md` y sus 7 preguntas abiertas, decisión de hospedaje y reorganización de carpetas (`web/`, `legacy/`, `docs/`, `android/`). Si se reorganiza `index.html` a `web/`, recordar mover también `assets/` manteniendo las rutas relativas `assets/tracking-record/...`.
- **Nota sobre Git:** el punto 8 (`index.html`) y esta actualización de `PROJECT_CONTEXT.md` / `README.md` se confirman al cierre de esta sesión y luego se hace `git push origin main` para actualizar GitHub Pages.
