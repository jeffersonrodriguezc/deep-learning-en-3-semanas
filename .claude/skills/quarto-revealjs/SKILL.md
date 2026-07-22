---
name: quarto-revealjs
description: >
  Crea, mejora y renderiza slides RevealJS en Quarto (.qmd) para el curso
  "Deep Learning Práctico en 3 Semanas". Úsalo cuando el usuario pida crear
  slides para una semana nueva, mejorar slides existentes, añadir una sección,
  convertir notas/outlines a slides, o renderizar y publicar. Actívalo también
  ante frases como "crea los slides de semana X", "mejora los slides", "añade
  una sección sobre Y", "renderiza" o "publica en GitHub Pages".
---

# Quarto RevealJS — Deep Learning en 3 Semanas

Skill para generar y mejorar presentaciones RevealJS del curso.
- **Repo:** `https://github.com/jeffersonrodriguezc/deep-learning-en-3-semanas`
- **Site:** `https://jeffersonrodriguezc.github.io/deep-learning-en-3-semanas/`

Lee `references/yaml-options.md` para opciones YAML avanzadas y
`references/scss-themes.md` para personalizar temas visuales por institución.

---

## Arquitectura del repositorio

```
deep-learning-en-3-semanas/
├── _quarto.yml                     ← sitio maestro: navbar + defaults revealjs (footer/theme del perfil BASE)
├── _quarto-uide-ds.yml             ← override de perfil: UIDE Ciencia de Datos (académico+aplicado)
├── _quarto-uide-dl.yml             ← override de perfil: UIDE Deep Learning (solo aplicado)
├── _quarto-montevideo.yml          ← override de perfil: Universidad de Montevideo (más aplicada)
├── index.qmd
├── introduccion_curso/
│   ├── index.qmd
│   └── slides/slides_introduccion_curso.qmd   ← bienvenida, instructor, hoja de ruta
├── introduccion_materia/
│   ├── index.qmd
│   ├── dl_timeline.png, ml_vs_dl.png
│   └── slides/slides_introduccion_materia.qmd ← qué es DL (universal) + contenido exclusivo por perfil
├── semana_1/
│   ├── index.qmd
│   └── slides/
│       ├── slides_semana_1.qmd     ← FUENTE MAESTRA con divs content-visible/content-hidden
│       ├── nn-animation.html       ← recursos interactivos junto al .qmd
│       ├── perceptron_estructura_basica.html
│       └── perceptron_clasificador_basico.html
├── semana_2/slides/slides_semana_2.qmd   ← (pendiente de crear)
├── semana_3/slides/slides_semana_3.qmd   ← (pendiente de crear)
│
├── params/                          ← metadata de referencia (institución, programa, color) usada al escribir los _quarto-<profile>.yml
│   ├── uide-ds.yml
│   ├── uide-dl.yml
│   └── montevideo.yml
└── assets/
    ├── uide.scss                    ← tema visual UIDE (compartido por uide-ds y uide-dl)
    ├── montevideo.scss              ← tema visual Montevideo
    └── logos/
        ├── uide.png
        └── montevideo.png
```

**Regla fundamental:** el contenido siempre se escribe en el archivo maestro
(`semana_N/slides/slides_semana_N.qmd`, `introduccion_curso/...`, `introduccion_materia/...`).
No existen carpetas `uide/` ni `montevideo/` con copias del contenido — **no se necesita
`{{< include >}}` de nada.** Una sola fuente por sección; lo que cambia entre
instituciones es (a) qué tan visible es cada bloque de contenido, resuelto con
divs `content-visible`/`content-hidden` + `when-profile`, y (b) el branding
(logo/color/footer), resuelto por el `_quarto-<profile>.yml` activo.

---

## YAML base real del curso

Basado en el archivo existente de semana 1. Usar como punto de partida exacto.

```yaml
---
title: "Semana N: Título"
subtitle: "Curso Práctico de Deep Learning en 3 Semanas"
author: "Jefferson Rodriguez"
format:
  revealjs:
    sanitize: false          # permite iframes y HTML embebido
    self-contained: false    # los recursos HTML viven como archivos separados
    incremental: true        # listas se revelan paso a paso por defecto
    theme: default
    slide-number: true
    show-slide-number: all
    mouse-wheel: true
    transition: fade
    footer: "Deep Learning en 3 Semanas - Jefferson Rodríguez"
    dependencies:
      - src: https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.min.js
        defer: true
editor: visual
resources:
  - nombre-animacion.html      # listar TODOS los HTML interactivos usados
---
```

**Notas importantes:**
- `self-contained: false` + `resources:` es el patrón para iframes locales
- Cada HTML interactivo debe estar listado en `resources:` Y existir junto al .qmd
- `sanitize: false` es obligatorio para que funcionen los iframes

---

## Patrones de slides reales del curso

### Separador de slide
Usar `---` para separar slides (no solo `##`). Los headings `###` y `##` ambos crean slides.

```markdown
---

### Título de la Slide

Contenido aquí.

---
```

### Iframe interactivo externo (ej. TensorFlow Playground)

```markdown
### Explorando DL: Un Vistazo Interactivo

```{=html}
<iframe src="https://playground.tensorflow.org/..." 
  width="100%" height="100%" frameborder="0" allowfullscreen="true">
</iframe>
```
```

### Iframe local (animación propia HTML/p5.js)

Los archivos HTML van en la misma carpeta que el .qmd y se listan en `resources:`.

```markdown
### Estructura de la Red Neuronal

```{=html}
<iframe
  src="nn-animation.html"
  width="800" height="500"
  style="display:block; margin:0 auto; border:none; overflow:hidden;"
  scrolling="no" allowfullscreen>
</iframe>
```
```

### Fragment con iframe + math (patrón perceptrón)

```markdown
### Perceptrón: El componente fundamental

::: {.fragment}
<div style="text-align:center; margin:1em 0;">
  <iframe src="perceptron_estructura_basica.html"
    width="450" height="200"
    style="border:none; margin:auto; display:block;"
    scrolling="no"></iframe>
</div>
:::

::: {.fragment}
$$
f(\mathbf{x}) = \sum_{i=1}^n w_i\,x_i + b
$$
:::
```

### Sección separadora

```markdown
---

# Nombre de Sección {background-color="#1a3a5c"}

---
```

### Slide con bullets incrementales (hereda `incremental: true` del YAML)

```markdown
### ¿Qué veremos esta semana?

* Punto uno que se revela solo
* Punto dos
* Punto tres
```

### Slide con bullets NO incrementales (override local)

```markdown
### Comparación de Arquitecturas

::: {.nonincremental}
- CNN: datos espaciales
- RNN: datos secuenciales
- Transformer: atención global
:::
```

### Math display

```markdown
$$
\mathcal{L} = -\frac{1}{N}\sum_{i=1}^N y_i \log(\hat{y}_i)
$$
```

### Código Python con echo

````markdown
### Implementación

```{python}
#| echo: true
#| code-line-numbers: "|2|5|8"

def forward(X, W, b):
    z = np.dot(X, W) + b
    return sigmoid(z)
```
````

---

## Sistema de visibilidad por perfil (content-visible / content-hidden)

**Sin div → la slide se ve siempre, en los 4 perfiles (base, uide-ds, uide-dl, montevideo).**
Solo se envuelve una sección cuando necesita comportarse distinto en algún perfil.

Los 4 perfiles activos hoy: `base` (sin `--profile`, es la versión completa de
referencia), `uide-ds`, `uide-dl`, `montevideo`.

```markdown
---

:::: {.content-hidden .academico when-profile="uide-dl,montevideo"}

### Derivación del gradiente

Contenido matemático denso. Se ve en base y uide-ds; se oculta en uide-dl y montevideo.

::::

---

### Introducción a la semana

Sin div — se ve en los 4 perfiles.

---

:::: {.content-visible when-profile="montevideo"}

### Caso de negocio: DL aplicado a scoring crediticio

Contenido exclusivo, solo existe para Montevideo.

::::

---
```

**Reglas de anidado (Pandoc):** el fence exterior necesita MÁS colones que
cualquier fence anidado dentro (ej. una tabla `.nonincremental` o un `.fragment`
dentro de una sección condicionada). Usa `::::` (4) por fuera cuando adentro
hay un `:::` (3) de otro div — si no, Pandoc cierra el div exterior de más.

| Necesito que... | Div a usar |
|---|---|
| ...se vea en TODOS los perfiles | ningún div (default) |
| ...se oculte solo en algunos perfiles | `.content-hidden` + `when-profile="perfil1,perfil2"` (lista = donde se OCULTA) |
| ...se vea solo en algunos perfiles | `.content-visible` + `when-profile="perfil1,perfil2"` (lista = donde se VE) |

**Activar un perfil al renderizar:**
```bash
quarto render semana_1/slides/slides_semana_1.qmd --profile montevideo
# o vía variable de entorno:
QUARTO_PROFILE=montevideo quarto render semana_1/slides/slides_semana_1.qmd
```

---

## Perfiles por institución

Cada perfil es un archivo `_quarto-<profile>.yml` en la raíz del proyecto,
que Quarto fusiona sobre `_quarto.yml` cuando se activa con `--profile`.
Solo contiene lo que CAMBIA (branding); nunca contenido.
Los `params/*.yml` existentes se conservan como metadata de referencia
humana (de dónde salieron estos valores) pero Quarto no los lee directamente.

### `_quarto-uide-ds.yml` — UIDE Maestría Ciencia de Datos con mención en IA
```yaml
website:
  title: "Neural Networks y Deep Learning · UIDE"
format:
  revealjs:
    theme: [default, assets/uide.scss]
    logo: assets/logos/uide.png
    footer: "Neural Networks y Deep Learning · UIDE · Jefferson Rodríguez"
```
Ve contenido `academico` y `aplicado` (es decir: no oculta nada tagueado como academico).

### `_quarto-uide-dl.yml` — UIDE Maestría Deep Learning
```yaml
website:
  title: "Deep Learning Aplicado · UIDE"
format:
  revealjs:
    theme: [default, assets/uide.scss]
    logo: assets/logos/uide.png
    footer: "Deep Learning Aplicado · UIDE · Jefferson Rodríguez"
```
Solo aplicado: cualquier bloque `.content-hidden` que liste `uide-dl` en su
`when-profile` se oculta aquí.

### `_quarto-montevideo.yml` — Universidad de Montevideo
```yaml
website:
  title: "Deep Learning · Universidad de Montevideo"
format:
  revealjs:
    theme: [default, assets/montevideo.scss]
    logo: assets/logos/montevideo.png
    footer: "Deep Learning · Universidad de Montevideo · Jefferson Rodríguez"
```
La más aplicada, y además tiene contenido EXCLUSIVO (marcado
`.content-visible when-profile="montevideo"`) que ningún otro perfil ve.

---

## Flujo de trabajo completo

### Escribir contenido nuevo (siempre en el archivo maestro)
1. Editar `semana_N/slides/slides_semana_N.qmd` (o `introduccion_curso/...`,
   `introduccion_materia/...`).
2. Si la slide debe verse distinto según institución, envolverla en
   `.content-hidden`/`.content-visible` + `when-profile` (ver sección anterior).
   Si no, no tocar nada — se ve en todos.
3. Si hay HTML interactivo nuevo, añadirlo a `resources:` del YAML del archivo.

### Renderizar para una institución
```bash
# Solo semana 1 para Montevideo
quarto render semana_1/slides/slides_semana_1.qmd --profile montevideo

# Todo el sitio para UIDE Ciencia de Datos
quarto render --profile uide-ds

# Todo el sitio base (sin profile = versión completa de referencia)
quarto render
```

### Publicar
```bash
quarto render                        # sitio base
quarto render --profile uide-ds      # (si se publica una versión UIDE-DS aparte)
quarto render --profile uide-dl
quarto render --profile montevideo
git add -A
git commit -m "feat: semana N actualizada"
git push origin main
```
Nota: publicar varias versiones institucionales en GitHub Pages requiere
decidir dónde vive cada build (subcarpetas de salida distintas vía
`output-dir` por perfil, o sitios/repos separados) — pendiente de definir
cuando toque publicar Montevideo por primera vez.

### Añadir semana nueva (ej. semana 2)
1. Crear `semana_2/slides/slides_semana_2.qmd` con el contenido, envolviendo
   en `content-hidden`/`content-visible` solo lo que diverja por institución.
2. Crear `semana_2/index.qmd` (mismo patrón que `semana_1/index.qmd`).
3. Renderizar con cada `--profile` y verificar visualmente.

### Añadir contenido exclusivo de una institución (ej. "Estructuración de proyectos" en Montevideo)
1. Escribirlo directamente en el archivo maestro de la sección que le
   corresponda (ej. `introduccion_materia/slides/slides_introduccion_materia.qmd`).
2. Envolver en `:::: {.content-visible when-profile="montevideo"} ... ::::`
   (o `.content-hidden` listando los perfiles que NO deben verlo — ambas
   formas son válidas, usar la que resulte más legible según cuántos
   perfiles quedan afuera vs. adentro).

---

## Pitfalls críticos de este repo

- `self-contained: false` + `resources:` es el patrón correcto para iframes locales.
- `sanitize: false` debe estar en el YAML de cada archivo maestro que use
  iframes, o no renderizan.
- Los HTML interactivos (p5.js, animaciones) viven junto a su `.qmd`.
- **No fijar `theme:` ni `footer:` en el YAML de un archivo maestro** — deben
  heredarse de `_quarto.yml` (base) o del `_quarto-<profile>.yml` activo.
  Si un `.qmd` los fija localmente, bloquea el override institucional.
- **Anidado de fences (`:::`):** el div exterior siempre necesita más colones
  que cualquier div anidado adentro. Con `.content-hidden`/`.content-visible`
  envolviendo secciones que ya tienen `.fragment` o `.nonincremental` adentro,
  usar `::::` (4) por fuera y `:::` (3) para el div interior existente.
- No existen carpetas `uide/` ni `montevideo/` — si aparecen en una sesión
  vieja de Claude Code, es plan obsoleto; ignorar y usar `_quarto-<profile>.yml`.
