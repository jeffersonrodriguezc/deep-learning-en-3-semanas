# Quarto RevealJS — Custom SCSS Themes

Quarto uses SCSS for custom theming. Create a `.scss` file alongside your `.qmd` and reference it in YAML:

```yaml
format:
  revealjs:
    theme: [default, assets/academic.scss]
```

The `[default, custom.scss]` pattern layers your overrides on top of the built-in theme.

---

## Academic Research Template (`assets/academic.scss`)

```scss
/*-- scss:defaults --*/

// Typography
$font-family-sans-serif: "Source Sans Pro", "Helvetica Neue", sans-serif !default;
$font-family-monospace: "Fira Code", "Courier New", monospace !default;
$presentation-font-size-root: 32px !default;

// Color palette (adjust to your institution)
$primary:   #1a3a5c !default;   // dark navy — titles, links
$secondary: #4a90d9 !default;   // medium blue — accents
$light:     #f4f6f9 !default;   // slide background
$dark:      #1e1e2e !default;   // text

// Slide dimensions (standard 16:9)
$presentation-slide-text-align: left !default;
$heading-color: $primary !default;
$link-color: $secondary !default;

/*-- scss:rules --*/

// Title slide
.reveal .slide-background-content {
  background-color: $light;
}

.reveal h1, .reveal h2 {
  font-weight: 600;
  letter-spacing: -0.02em;
}

.reveal h1 {
  font-size: 1.8em;
  color: $primary;
  border-bottom: 3px solid $secondary;
  padding-bottom: 0.2em;
}

.reveal h2 {
  font-size: 1.3em;
  color: $primary;
}

// Code blocks
.reveal pre {
  border-left: 4px solid $secondary;
  background-color: #f0f4f8;
}

// Highlighted text (==text==)
mark {
  background-color: lighten($secondary, 35%);
  padding: 0 0.15em;
  border-radius: 3px;
}

// Footer
.reveal .footer {
  font-size: 0.55em;
  color: lighten($dark, 40%);
}

// Table styling
.reveal table {
  font-size: 0.85em;
  border-collapse: collapse;
}
.reveal table th {
  background-color: $primary;
  color: white;
  padding: 0.4em 0.8em;
}
.reveal table td {
  padding: 0.3em 0.8em;
  border-bottom: 1px solid #ddd;
}

// Callout box (use as ::: {.callout})
.callout {
  border-left: 4px solid $secondary;
  background: lighten($secondary, 42%);
  padding: 0.6em 1em;
  border-radius: 4px;
  font-size: 0.9em;
}
```

---

## Teaching Template (`assets/teaching.scss`)

```scss
/*-- scss:defaults --*/

$font-family-sans-serif: "Inter", "Segoe UI", sans-serif !default;
$presentation-font-size-root: 30px !default;
$primary:   #2d6a4f !default;   // green — educational feel
$secondary: #52b788 !default;   // lighter green accent
$light:     #f8f9fa !default;
$dark:      #212529 !default;

$heading-color: $primary !default;
$link-color: #0077b6 !default;

/*-- scss:rules --*/

.reveal h1, .reveal h2 {
  font-weight: 700;
  color: $primary;
}

// Highlight important terms
mark {
  background-color: #fff3bf;
  border-radius: 3px;
  padding: 0 0.1em;
}

// Code — show-code teaching style
.reveal pre {
  font-size: 0.75em;
  max-height: 500px;
}

// Incremental list items
.reveal .fragment {
  opacity: 0.3;
}
.reveal .fragment.visible {
  opacity: 1;
}

// Definition boxes
.definition {
  background: lighten($secondary, 38%);
  border-left: 5px solid $primary;
  padding: 0.5em 1em;
  border-radius: 4px;
  margin: 0.5em 0;
  font-size: 0.9em;
}
```

Usage of `.definition` in a slide:

```markdown
::: {.definition}
**Watermarking** is the process of embedding imperceptible data into a carrier signal.
:::
```

---

## Tips

- Run `quarto preview slides.qmd` to see SCSS changes live.
- Use browser DevTools (F12 → Inspector) to find CSS class names for overriding.
- Keep SCSS files in an `assets/` subdirectory alongside the `.qmd`.
- The `/*-- scss:defaults --*/` section sets SCSS variables; `/*-- scss:rules --*/` adds raw CSS.
