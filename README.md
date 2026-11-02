# kimai-theme

A shadcn/ui-style reskin for [Kimai](https://www.kimai.org) (zinc palette, light and dark, Inter font) with
[Lucide](https://lucide.dev) icons in place of Font Awesome. It is a single stylesheet for the **Custom CSS plugin**
(`CustomCSSBundle`), plus the font files that stylesheet references. No Twig overrides, no Kimai code changes.

## What you get

- `dist/custom-css.css`: the whole theme. Re-points Tabler's `--tblr-*` variables at shadcn's tokens and restyles
  buttons, cards, tables, the navbar/sidebar, dropdowns, modals and alerts. Follows Kimai's own light/dark switch.
- `dist/fonts/inter/`: self-hosted Inter (woff2, SIL Open Font License), so there is no request to Google Fonts.
- `dist/fonts/lucide/`: the Lucide icon font (ISC licence, see `LICENSE` there).

Kimai renders every icon as `<i class="fas fa-trash-alt">`. The stylesheet swaps the glyph through the `::before`
pseudo-element, so no markup changes: each mapped Font Awesome class switches to the Lucide font and codepoint.
Icons without a mapping keep their Font Awesome glyph.

## Install

1. Install the Custom CSS plugin in `var/plugins/CustomCSSBundle`.
2. Make these files available inside the Kimai container (bind mounts shown for Docker Compose):

   ```yaml
   volumes:
     - ./dist/custom-css.css:/opt/kimai/var/data/custom-css.css:ro
     - ./dist/fonts/inter:/opt/kimai/public/fonts/inter:ro
     - ./dist/fonts/lucide:/opt/kimai/public/fonts/lucide:ro
   ```

   The plugin reads its rules from `var/data/custom-css.css`. Mounting the file read-only keeps it version-controlled
   instead of editable from the plugin's admin screen.
3. Recreate the container after changing `custom-css.css`. A single-file bind mount is resolved to an inode when the
   container starts, so replacing the file on the host is not seen until then (`docker compose up -d --force-recreate kimai`).
   The two font directories do not have this problem.

The font URLs in the stylesheet are root-relative (`/fonts/inter/...`, `/fonts/lucide/...`), matching the mounts above.

## Develop

Sources are in `src/`, the committed `dist/` is generated:

```bash
npm install
npm run build
```

- `src/theme.css`: the reskin.
- `src/icons-map.json`: Font Awesome class name (without `fa-`) to Lucide icon name. The build fails on a Lucide
  name that does not exist.
- `src/fonts/inter/`: Inter font files.

To map another icon, add it to `src/icons-map.json` and rebuild. To find the Font Awesome class an icon uses,
inspect it in the browser. Bumping `lucide-static` in `package.json` can rename or remove icons, and the build will
say which.

## Notes

- Tested against Kimai 2 (Tabler UI, Font Awesome 6 Free). Tabler variable names or Kimai markup can change between
  major versions, so check the result after upgrading Kimai.
- Lucide icons are stroke-based, so Font Awesome's solid (`fas`) and outline (`far`) variants look the same.
- The Lucide font contains every Lucide icon (about 300 KB) and is cached by the browser after the first load.
