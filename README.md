# avaltecnico.com — web de marca personal de Carlos Jiménez

Sitio estático hecho con **Jekyll**, alojado en **GitHub Pages** y editable desde
**[Pages CMS](https://pagescms.org)** sin tocar código.

---

## 1. Puesta en marcha (una sola vez)

### 1.1 Subir el proyecto a GitHub

```bash
git init
git add .
git commit -m "Primera versión de avaltecnico.com"
git branch -M main
git remote add origin https://github.com/<tu-usuario>/avaltecnico.git
git push -u origin main
```

> El repositorio puede ser **privado**: GitHub Pages y Pages CMS funcionan igual.

### 1.2 Activar GitHub Pages

En el repositorio → **Settings → Pages → Build and deployment → Source: GitHub Actions**.

El fichero `.github/workflows/deploy.yml` ya está preparado: cada `push` a `main`
—incluidos los que haga Pages CMS al guardar— publica el sitio automáticamente.

### 1.3 Dominio propio (avaltecnico.com)

1. En **Settings → Pages → Custom domain**, escribe `avaltecnico.com` y guarda.
   (El fichero `CNAME` del repositorio ya lo declara.)
2. En tu proveedor de DNS crea:

   | Tipo  | Nombre | Valor |
   |-------|--------|-------|
   | A     | `@`    | `185.199.108.153` |
   | A     | `@`    | `185.199.109.153` |
   | A     | `@`    | `185.199.110.153` |
   | A     | `@`    | `185.199.111.153` |
   | CNAME | `www`  | `<tu-usuario>.github.io.` |

3. Cuando el DNS propague, marca **Enforce HTTPS**.

### 1.4 Conectar Pages CMS

1. Entra en <https://app.pagescms.org> con tu cuenta de GitHub.
2. Autoriza la aplicación **solo para este repositorio**.
3. Selecciona el repositorio y la rama `main`.

Pages CMS lee `.pages.yml` y te muestra el panel de edición ya montado:

| Sección del CMS | Qué edita |
|---|---|
| **Página de inicio** | Todos los bloques de la home (portada, dolor, servicios, proceso, independencia, sobre mí, FAQ, CTA) |
| **Página · Servicios / Sobre mí / Independencia / Contacto** | Título, entradilla, SEO y contenido |
| **Artículos** | El blog (`_posts`) |
| **Páginas legales** | Aviso legal y privacidad |
| **Ajustes del sitio** | Marca, menú, contacto, analítica, enlaces del pie |
| **Media** | Tus fotografías (`assets/img`) |

Cada vez que guardas en el CMS se hace un commit en `main` y el sitio se
republica solo en 1–2 minutos.

---

## 2. Cómo poner tus fotografías

Ahora mismo hay **placeholders**: si un campo de imagen está vacío, el sitio
muestra un marcador de posición en lugar de romperse.

Formatos recomendados:

| Dónde | Formato | Tamaño mínimo | Campo en el CMS |
|---|---|---|---|
| Portada (home) | vertical 3:4 | 900×1200 px | Página de inicio → Portada → *Fotografía vertical* |
| Bloque «Quién te avala» (home) | cuadrada 1:1 | 900×900 px | Página de inicio → Sección 6 → *Fotografía cuadrada* |
| Página «Sobre mí» | horizontal 16:9 | 1600×900 px | Página · Sobre mí → *Fotografía horizontal* |
| Compartir en redes | 1200×630 px | — | sustituye `assets/img/og-default.png` |

Desde Pages CMS: abre la página, pulsa el campo de imagen, sube la foto y guarda.
No hay que tocar nada más.

**Consejo:** exporta en JPG a ~80 % de calidad y menos de 400 KB por imagen.

---

## 3. Antes de publicar: pendientes

- [ ] Sustituir los datos entre corchetes en `legal/aviso-legal.md` y `legal/privacidad.md` (NIF, domicilio, plazos).
- [ ] Poner tu URL real de LinkedIn en **Ajustes del sitio → Datos de contacto**.
- [ ] Decidir si el correo de contacto es `hola@avaltecnico.com` y crearlo.
- [ ] Opcional: enlace de agenda (Calendly/Cal.com) → se muestra en el pie y en Contacto.
- [ ] Opcional: crear un formulario en [Formspree](https://formspree.io) y pegar el endpoint
      en **Ajustes → Datos de contacto → Endpoint de Formspree**. Mientras esté vacío, la
      página de contacto muestra un botón de correo en lugar del formulario.
- [ ] Opcional: analítica sin cookies con [Plausible](https://plausible.io) → escribe el dominio en **Ajustes → Analítica**.
- [ ] Añadir precios concretos cuando estén decididos (Página de inicio → Sección 3 y Página · Servicios).

---

## 4. Trabajar en local (opcional)

Requiere Ruby ≥ 3.1. En este equipo está en `/opt/homebrew/opt/ruby/bin`:

```bash
export PATH=/opt/homebrew/opt/ruby/bin:$PATH
bundle install
bundle exec jekyll serve
# → http://127.0.0.1:4000
```

---

## 5. Estructura del proyecto

```
_config.yml              Configuración de Jekyll (layouts, permalinks, SEO)
.pages.yml               Configuración del panel de Pages CMS
index.md                 Contenido completo de la home (en el front matter)
servicios.md             Página de servicios
sobre-mi.md              Página sobre mí
independencia.md         Política de no conflicto de interés
contacto.md              Página de contacto
legal/                   Aviso legal y privacidad
_posts/                  Artículos del blog
_data/site.yml           Marca, menú, contacto, pie, analítica
_layouts/                Plantillas (home, page, post, contacto, default)
_includes/               Cabecera, pie, CTA, fotos, fechas en castellano
assets/css/main.css      Todo el diseño (modo oscuro)
assets/img/              Imágenes y placeholders
.github/workflows/       Publicación automática en GitHub Pages
```

## 6. Notas de diseño

- Paleta: grafito frío + violeta eléctrico (acento) + verde de validación (los checks).
  Tipografías: *Space Grotesk* (títulos), *Inter* (texto) y *JetBrains Mono*
  (antetítulos, etiquetas, precios y metadatos).
- Logotipo: un check dentro de un corchete cuadrado — «validado» en lenguaje de
  programador, que es exactamente lo que significa un aval. Vive en
  `_includes/logo.html` (SVG en línea, hereda los colores del tema) y hay copias
  para el favicon (`assets/img/favicon.svg`) y la imagen para redes.
- El sitio es **solo modo oscuro**: no depende de la preferencia del sistema del
  visitante, se ve igual para todo el mundo. `color-scheme: dark` hace que los
  campos de formulario y las barras de desplazamiento acompañen al diseño.
- Todo el copy sale del posicionamiento definido en `posicionamiento-estrategia.md`:
  la promesa es **visibilidad**, nunca el resultado final del proyecto, y la
  independencia respecto a la ejecución es un argumento de venta explícito.
