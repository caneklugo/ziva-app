# 🚀 Guía de Despliegue en GitHub Pages (Ziva)

Este proyecto está 100% listo para publicarse en **GitHub Pages**. Todos los archivos son estáticos y usan rutas relativas, por lo que funcionará de forma inmediata en `https://<tu-usuario>.github.io/<nombre-del-repo>/`.

---

## ❓ ¿El repositorio tiene que ser Público o puede ser Privado?

- **En cuentas gratuitas de GitHub (GitHub Free):**
  **SÍ, debe ser PÚBLICO**. En cuentas gratuitas, GitHub Pages solo se activa en repositorios públicos. Si lo pones privado, la opción de Pages en *Settings* aparecerá bloqueada con el mensaje *"Upgrade to GitHub Pro to publish Pages from private repositories"*.
  
- **En cuentas con GitHub Pro, GitHub Team o Student Developer Pack:**
  **SÍ se puede dejar PRIVADO**. GitHub Pages te permitirá desplegar la página web aunque el código fuente permanezca privado.

> 💡 **Recomendación para Enactus:** Déjalo **Público**. De este modo, los jueces y directores podrán abrir tanto la landing page como el simulador desde sus teléfonos y computadoras con un simple clic sin problemas de permisos ni inicios de sesión.

---

## Paso a Paso: Subir y Desplegar en menos de 2 minutos

### Paso 1: Crea el repositorio en GitHub
1. Entra a [github.com/new](https://github.com/new).
2. Nombre del repositorio: por ejemplo `ziva-app` o `prototipo-enactus`.
3. Selecciona **Public**.
4. **NO** marques "Add a README file" ni `.gitignore` (ya están creados en este proyecto).
5. Haz clic en **Create repository**.

### Paso 2: Conecta y sube tus archivos desde la terminal
Abre tu terminal en la carpeta del proyecto y corre estos comandos (sustituye `<tu-usuario>` y `<nombre-repo>` con tus datos de GitHub):

```bash
git add .
git commit -m "feat: landing page y prototipo Ziva para Enactus"
git remote add origin https://github.com/<tu-usuario>/<nombre-repo>.git
git push -u origin main
```

*(Si ya tenías el remoto agregado previamente y solo quieres subir, solo usa `git push -u origin main`).*

---

### Paso 3: Activar GitHub Pages en el repositorio

1. En tu repositorio de GitHub, haz clic en la pestaña **Settings** (Ajustes) en la barra superior.
2. En el menú lateral izquierdo, ve a **Pages** (dentro de "Code and automation").
3. En **Build and deployment**:
   - **Source**: selecciona `Deploy from a branch`.
   - **Branch**: selecciona la rama `main` y la carpeta `/(root)`.
   - Haz clic en **Save** (Guardar).
4. Espera 1 minuto y recarga la página. Verás la URL activa en verde:
   > *"Your site is live at `https://<tu-usuario>.github.io/<nombre-repo>/`"*

---

## 📱 Enlaces disponibles una vez desplegado:

- **Landing Page principal:** `https://<tu-usuario>.github.io/<nombre-repo>/` (muestra la presentación de Enactus, estadísticas, catálogo de alertas y el smartphone interactivo).
- **Prototipo móvil a pantalla completa:** `https://<tu-usuario>.github.io/<nombre-repo>/app.html` (diseñado específicamente para abrirse directamente en teléfonos durante demostraciones y pitches).
