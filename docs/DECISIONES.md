# Decisiones de implementación

Notas sobre decisiones que no se explican solas al leer el código. Cada una
existe por una razón concreta; revertirlas sin leer esto rompe algo.

Última actualización: 2026-08-01.

---

## `locale` / `label` en vez de `languageCode` / `languageName`

**Dónde:** `hugo.toml`, bloques `[languages.en]` y `[languages.es]`.

Hugo 0.158.0 deprecó `languageCode` y `languageName` en favor de `locale` y
`label`. Con las claves viejas el build emite cuatro advertencias.

**Consecuencia:** el sitio ya no compila igual en Hugo < 0.158. Una versión
anterior no reconoce las claves nuevas y pierde el idioma del RSS y las
etiquetas del selector — sin fallar, solo generando HTML incompleto. Por eso
`HUGO_VERSION` en `.github/workflows/deploy.yml` está fijado en 0.164.0, la
misma versión que se usa en local. **No bajar esa versión.**

---

## El selector de idioma necesita un override; `translationKey` no basta

**Dónde:** `layouts/partials/header.html`, bloque del `lang-switch`.

PaperMod construye el selector con `site.Home.Translations` — las traducciones
del *home*, no de la página actual. Por diseño del tema, el selector siempre
lleva al home del otro idioma.

Agregar `translationKey` al front matter **no cambia eso**. Sirve para otras
cosas (el `hreflang`, la lista "Traducciones:" bajo el título), pero el header
se arregla solo tocando la plantilla. El override usa `.Translations` de la
página actual y cae al home cuando no hay equivalente, para no generar enlaces
muertos en páginas sin traducción como los tags.

---

## `layouts/partials/header.html` no es una copia del partial del tema

Además del cambio del selector, ese archivo tiene lógica propia para
normalizar las URLs del menú cuando el `baseURL` incluye una subcarpeta.

**Si hace falta actualizar el override, editar el bloque puntual.** Copiar el
`header.html` de PaperMod encima destruye esa lógica y rompe los enlaces del
menú.

---

## `#menu .active` en el CSS, no `.nav .active`

**Dónde:** `assets/css/extended/custom.css`.

El tema estiliza el ítem activo con `#menu .active`. Un selector de clase
pierde por especificidad contra un ID sin importar el orden de carga, así que
la regla propia tiene que usar el mismo ID. Parece redundante; no lo es.

---

## `--code-block-bg` se queda oscuro en modo claro

**Dónde:** `assets/css/extended/custom.css`.

`hugo.toml` usa `[markup.highlight] style = "monokai"`, un esquema de texto
claro sobre fondo oscuro. Si el fondo del bloque siguiera la paleta clara, los
tokens quedarían ilegibles. El tema hace lo mismo por el mismo motivo.

**Para tener bloques de código claros** hay que cambiar el `style` de Chroma en
`hugo.toml`, no esta variable.

---

## La etiqueta `mask-icon` va sin atributo `color`

**Dónde:** emitida por el tema en `head.html`; los iconos se configuran en
`hugo.toml`, `[params.assets]`.

PaperMod emite `<link rel="mask-icon">` sin `color` y sin forma de
parametrizarlo. Agregar la etiqueta con color desde `extend_head.html`
produciría **dos** etiquetas `mask-icon` y un comportamiento indefinido.

Se optó por dejarla sin color: Safari usa negro por defecto, y el color de
marca (`#16181B`) está a tres puntos del negro puro — invisible a 16 px. La
alternativa era un override completo de `head.html` (~200 líneas) por un
detalle imperceptible.

---

## Los iconos van en `[params.assets]` con rutas explícitas

El set generado usa nombres modernos (`favicon-96x96.png`, `favicon.svg`,
`site.webmanifest`) que no coinciden con los que PaperMod espera por defecto
(`favicon-16x16.png`, `favicon-32x32.png`). Sin las rutas explícitas, esas
etiquetas apuntan a archivos inexistentes.

Los atributos `sizes="16x16"` y `sizes="32x32"` del HTML son fijos en el tema y
ambos apuntan al PNG de 96×96. Es metadata imprecisa a propósito: los
navegadores escalan sin problema, y el `rel="icon" type="image/svg+xml"` que
agrega `extend_head.html` tiene prioridad en navegadores modernos.

---

## `params.label.text` en vez de acortar `title`

**Dónde:** `hugo.toml`, `[params.label]`.

El header muestra "Mauricio Guerrero" pero el `<title>`, el RSS y las metaetiquetas
siguen usando el título completo del idioma ("Mauricio Guerrero - Personal
Site"). Son dos audiencias distintas: el logo es identidad visual, el título es
lo que aparece en resultados de búsqueda y al compartir enlaces.

---

## El contenido de ejemplo está en `draft: true`, no borrado

**Dónde:** `content/*/projects/weather-cli.md`, `task-manager.md`,
`content/*/posts/how-to-write-about-projects.md`,
`what-makes-strong-technical-post.md`.

Venían del template. Se despublicaron en vez de eliminarse por si sirven de
base más adelante.

**Ojo:** `hugo server` los oculta salvo que se pase `-D`. Para saber qué se
publica de verdad, `hugo list published`.

---

## `extend_head.html` en vez de sobrescribir `head.html`

PaperMod no emite el icono SVG ni el enlace al `site.webmanifest`, y no tiene
parámetros para ellos. `extend_head.html` es el punto de extensión oficial del
tema, así que ambos se agregan ahí. Sobrescribir `head.html` completo habría
sido otro archivo de 200 líneas que revisar en cada actualización del tema.

---

## El CSS propio vive en `assets/css/extended/`

PaperMod concatena todo `css/extended/*.css` **después** del CSS del tema, así
que las reglas propias ganan por orden de carga sin necesidad de `!important`
ni de tocar `themes/`.

Las reglas de enlaces cubren `.post-content a` **y** `.entry-content a`: la
primera clase es la de los posts, la segunda la del home y los resúmenes de los
listados. Sin la segunda, el enlace de descarga del CV en el home se queda con
el estilo del tema.
