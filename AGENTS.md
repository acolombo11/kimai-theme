# kimai-theme

A stylesheet for Kimai's Custom CSS plugin: shadcn/ui-style reskin plus Lucide icons over Font Awesome. See
[README.md](README.md) for what it does and how to mount it.

## Layout

- `src/theme.css`: the reskin (hand-written).
- `src/icons-map.json`: Font Awesome class (without `fa-`) to Lucide icon name.
- `src/fonts/inter/`: Inter woff2 files.
- `scripts/build.mjs`: concatenates `theme.css` with the generated icon rules into `dist/custom-css.css` and copies the
  fonts into `dist/fonts/`.
- `dist/`: generated and **committed**, because consumers use it as is. There is no CI build.

## Rules

- Never edit `dist/` by hand. Change `src/` and run `npm run build`, then commit both.
- Keep this repo generic: no hostnames, domains, server paths, credentials or references to any particular
  deployment, in code, comments or docs. Describe usage in terms of the Custom CSS plugin and Docker mounts only.
- The Custom CSS plugin injects the stylesheet as an inline `<style>` after Tabler's own CSS, so plain selectors
  usually win. Use `!important` only against Tabler rules with higher specificity (for example the sidebar's
  `.navbar-expand-lg.navbar-vertical` box-shadow).
- A Lucide icon name that does not exist fails the build; check `node_modules/lucide-static/font/codepoints.json`.
