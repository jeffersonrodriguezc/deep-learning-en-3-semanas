# Quarto RevealJS — YAML Options Reference

## Layout & Navigation

| Key | Default | Notes |
|-----|---------|-------|
| `slide-number` | false | `true`, `c/t` (current/total), `h.v`, `h/v` |
| `show-slide-number` | `all` | `all`, `print`, `speaker` |
| `progress` | true | Bottom progress bar |
| `history` | true | Browser back button works |
| `navigation-mode` | `linear` | `linear`, `vertical`, `grid` |
| `controls` | true | Arrow navigation controls |
| `controls-layout` | `bottom-right` | Conflicts with `logo` |
| `center` | false | Center all slide content vertically |
| `center-title-slide` | true | Center title slide only |
| `scrollable` | false | Allow slide scrolling (disable auto-stretch if used) |

## Visual & Theme

| Key | Default | Notes |
|-----|---------|-------|
| `theme` | `default` | `beige`, `blood`, `dark`, `league`, `moon`, `night`, `serif`, `simple`, `sky`, `solarized` |
| `transition` | `none` | `none`, `fade`, `slide`, `convex`, `concave`, `zoom` |
| `background-transition` | `none` | Same values as `transition` |
| `highlight-style` | `default` | `github`, `a11y`, `monokai`, `zenburn` |
| `logo` | — | Path to image; appears bottom-right |
| `footer` | — | Footer text (supports markdown) |

## Content Behavior

| Key | Default | Notes |
|-----|---------|-------|
| `incremental` | false | All lists reveal step-by-step |
| `auto-stretch` | true | Images auto-fit slide height |
| `fig-align` | `default` | `center`, `left`, `right` |
| `fig-cap-location` | `bottom` | `top`, `bottom`, `margin` |

## Code

| Key | Default | Notes |
|-----|---------|-------|
| `code-fold` | false | Collapsible code blocks |
| `code-block-height` | `500px` | Max height before scrolling |

## Export & Portability

| Key | Default | Notes |
|-----|---------|-------|
| `embed-resources` | false | **Set true for sharing**; bundles all assets into one HTML |
| `self-contained` | false | Deprecated alias for `embed-resources` |

## Execute (global code options)

```yaml
execute:
  echo: false        # hide code
  warning: false     # hide warnings
  message: false     # hide messages
  cache: true        # cache results (useful for slow computations)
  freeze: auto       # don't re-execute unchanged chunks
```

## Chalkboard (teaching)

```yaml
format:
  revealjs:
    chalkboard: true           # enables drawing tools; press B to toggle
```

## Multiplex (live audience sync)

```yaml
format:
  revealjs:
    multiplex: true            # syncs presenter + audience slides via token
```

## Plugins

```yaml
revealjs-plugins:
  - pointer                    # laser pointer; install via quarto add quarto-ext/pointer
```
