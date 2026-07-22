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
├── _quarto.yml                     ← sitio maestro (no tocar navbar/theme)
├── index.qmd
├── semana_1/
│   ├── index.qmd
│   └── slides/
│       ├── slides_semana_1.qmd     ← FUENTE MAESTRA con todos los tags
│       ├── nn-animation.html       ← recursos interactivos junto al .qmd
│       ├── perceptron_estructura_basica.html
│       └── perceptron_clasificador_basico.html
├── semana_2/slides/slides_semana_2.qmd
├── semana_3/slides/slides_semana_3.qmd
│
├── uide/                           ← versión UIDE (include del base)
│   ├── _quarto.yml                 ← config UIDE
│   ├── index.qmd
│   └── semana_1/slides/slides.qmd  ← solo YAML + {{< include >}}
│
├── montevideo/                     ← versión Montevideo (include del base)
│   ├── _quarto.yml
│   ├── index.qmd
│   └── semana_1/slides/slides.qmd
│
├── params/
│   ├── uide-ds.yml
│   ├── uide-dl.yml
│   └── montevideo.yml
└── assets/
    └── logos/
        ├── uide.png
        └── montevideo.png
```

**Regla fundamental:** el contenido siempre se escribe en `semana_N/slides/slides_semana_N.qmd`.
Las carpetas `uide/` y `montevideo/` nunca contienen contenido propio — solo incluyen el base.

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

## Sistema de tags por audiencia

Cada slide con `###` o `##` puede llevar un tag de audiencia.
**Sin tag → se incluye siempre en todos los cursos.**

```markdown
### Derivación del gradiente {tags="academico"}

Contenido matemático denso.

---

### Demo en PyTorch {tags="aplicado"}

Contenido práctico.

---

### Introducción a la semana

Sin tag — va en todos los cursos.
```

| Tag | Cursos que lo incluyen |
|-----|----------------------|
| `academico` | UIDE Ciencia de Datos |
| `aplicado` | UIDE Ciencia de Datos, UIDE Deep Learning, Montevideo |
| sin tag | Todos |

---

## Perfiles por institución

### `params/uide-ds.yml` — UIDE Maestría Ciencia de Datos con mención en IA
```yaml
institucion: "Universidad Internacional del Ecuador"
programa: "Maestría en Ciencia de Datos con mención en IA"
curso: "Neural Networks y Deep Learning"
logo: "../../assets/logos/uide.png"
color_primario: "#003087"
footer: "Neural Networks y Deep Learning · UIDE · Jefferson Rodríguez"
audiencia: ["academico", "aplicado"]
```

### `params/uide-dl.yml` — UIDE Maestría Deep Learning
```yaml
institucion: "Universidad Internacional del Ecuador"
programa: "Maestría en Deep Learning"
curso: "Deep Learning Aplicado"
logo: "../../assets/logos/uide.png"
color_primario: "#003087"
footer: "Deep Learning Aplicado · UIDE · Jefferson Rodríguez"
audiencia: ["aplicado"]
```

### `params/montevideo.yml` — Universidad de Montevideo
```yaml
institucion: "Universidad de Montevideo"
programa: "Maestría en Ciencia de Datos · Facultad Empresarial y Economía"
curso: "Deep Learning"
logo: "../../assets/logos/montevideo.png"
color_primario: "#8B0000"
footer: "Deep Learning · UM · Jefferson Rodríguez"
audiencia: ["aplicado"]
```

---

## Archivos institucionales (include del base)

Las carpetas `uide/` y `montevideo/` contienen archivos mínimos que
solo personalizan el YAML e incluyen el contenido base con `{{< include >}}`.

### `uide/semana_1/slides/slides.qmd`
```yaml
---
title: "Semana 1: Fundamentos de Redes Neuronales"
subtitle: "Neural Networks y Deep Learning"
author: "Jefferson Rodriguez"
format:
  revealjs:
    sanitize: false
    self-contained: false
    incremental: true
    theme: [default, ../../../assets/uide.scss]
    slide-number: true
    show-slide-number: all
    mouse-wheel: true
    transition: fade
    footer: "Neural Networks y Deep Learning · UIDE · Jefferson Rodríguez"
    logo: ../../../assets/logos/uide.png
    dependencies:
      - src: https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.min.js
        defer: true
resources:
  - ../../../semana_1/slides/nn-animation.html
  - ../../../semana_1/slides/perceptron_estructura_basica.html
  - ../../../semana_1/slides/perceptron_clasificador_basico.html
audiencia: ["academico", "aplicado"]
---

{{< include ../../../semana_1/slides/slides_semana_1.qmd >}}
```

Mismo patrón para `montevideo/semana_1/slides/slides.qmd` cambiando
footer, logo, scss y audiencia.

---

## Flujo de trabajo completo

### Escribir contenido nuevo (siempre en el base)
1. Editar `semana_N/slides/slides_semana_N.qmd`
2. Taggear cada slide nueva con `{tags="academico"}`, `{tags="aplicado"}` o sin tag
3. Si hay HTML interactivo nuevo, añadirlo a `resources:` del YAML base

### Renderizar para una institución
```bash
# Solo semana 1 para UIDE
quarto render uide/semana_1/slides/slides.qmd

# Todo el sitio UIDE
quarto render uide/

# Todo el sitio Montevideo
quarto render montevideo/
```

### Publicar
```bash
quarto render          # sitio maestro
quarto render uide/
quarto render montevideo/
git add -A
git commit -m "feat: semana N actualizada"
git push origin main
```

### Añadir semana nueva (ej. semana 2)
1. Crear `semana_2/slides/slides_semana_2.qmd` con contenido taggeado
2. Crear `uide/semana_2/slides/slides.qmd` (solo YAML + include)
3. Crear `montevideo/semana_2/slides/slides.qmd` (solo YAML + include)
4. Renderizar y verificar

---

## Pitfalls críticos de este repo

- `self-contained: false` + `resources:` es el patrón correcto para iframes locales.
  En los archivos institucionales, las rutas en `resources:` deben ser relativas
  al `.qmd` institucional, apuntando a los HTML en `semana_N/slides/`.
- `sanitize: false` debe estar en TODOS los YAMLs (base e institucionales)
  o los iframes no renderizan.
- Los HTML interactivos (p5.js, animaciones) viven en `semana_N/slides/` —
  nunca duplicarlos en las carpetas institucionales.
- Al renderizar un archivo institucional, Quarto resuelve el `{{< include >}}`
  desde la ruta del archivo institucional — verificar que las rutas relativas
  de iframes dentro del base sean correctas desde esa perspectiva.
