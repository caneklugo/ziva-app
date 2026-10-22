# 🚀 Guía de Despliegue en GitHub Pages (en menos de 2 minutos)

Este proyecto está 100% listo para publicarse en **GitHub Pages**. Todos los archivos son estáticos y usan rutas relativas, por lo que funcionará de forma inmediata en `https://<tu-usuario>.github.io/<nombre-del-repo>/`.

---

## Opción 1: Crear el repositorio en GitHub y subir los cambios (Recomendada)

### Paso 1: Crea el repositorio en GitHub
1. Entra a [github.com/new](https://github.com/new).
2. Ponle un nombre al repositorio (por ejemplo: `prototipo-enactus` o `senal-app`).
3. Elige si es **Público** (necesario para GitHub Pages en cuentas gratuitas).
4. **NO** marques "Add a README file" ni `.gitignore` (ya están creados localmente).
5. Haz clic en **Create repository**.

### Paso 2: Conecta y sube tus archivos desde la terminal
Abre tu terminal en la carpeta del proyecto y corre estos comandos (sustituye `<tu-usuario>` y `<nombre-repo>` por los tuyos):

```bash
git add .
git commit -m "feat: landing page y prototipo rediseñado para Enactus"
git remote add origin https://github.com/<tu-usuario>/<nombre-repo>.git
git push -u origin main
```

---

## Paso 3: Activar GitHub Pages en el repositorio

1. En tu repositorio de GitHub, haz clic en la pestaña **Settings** (Ajustes) en la parte superior derecha.
2. En el menú lateral izquierdo, busca la sección **Pages** (dentro de "Code and automation").
3. En **Build and deployment**:
   - **Source**: selecciona `Deploy from a branch`.
   - **Branch**: selecciona la rama `main` y la carpeta `/(root)`.
   - Haz clic en **Save** (Guardar).
4. Espera 1 minuto y recarga la página. Verás un mensaje en verde:
   > *"Your site is live at `https://<tu-usuario>.github.io/<nombre-repo>/`"*

---

## 📱 Enlaces disponibles una vez desplegado:

- **Landing Page principal:** `https://<tu-usuario>.github.io/<nombre-repo>/` (muestra la presentación de Enactus y el prototipo interactivo integrado en el marco de celular).
- **Prototipo móvil en pantalla completa:** `https://<tu-usuario>.github.io/<nombre-repo>/app.html` (ideal para abrirlo directamente desde el celular en una presentación o evaluación).
