# Hyas Tailwind CSS

Official Tailwind CSS integration for Hyas.

## Status

[![npm (scoped)](https://img.shields.io/npm/v/@hyas/tailwindcss?style=flat-square)](https://www.npmjs.com/package/@hyas/tailwindcss)

## Installation

```bash
npm i -D @hyas/tailwindcss
```

## Setup

Add mounts to `./config/_default/module.toml`:

```toml
[[mounts]]
  source = "node_modules/@hyas/tailwindcss/assets"
  target = "assets"

[[mounts]]
  source = "node_modules/@hyas/tailwindcss/layouts"
  target = "layouts"

[[mounts]]
  source = "assets"
  target = "assets"

[[mounts]]
  source = "layouts"
  target = "layouts"

[[mounts]]
  source = "hugo_stats.json"
  target = "assets/watching/hugo_stats.json"
```

Set cache busters in `./config/_default/hugo.toml`:

```toml
[build]
  writeStats = true
  [[build.cachebusters]]
    source = "assets/watching/hugo_stats\\.json"
    target = "styles\\.css"
  [[build.cachebusters]]
    source = "(postcss|tailwind)\\.config\\.js"
    target = "css"
  [[build.cachebusters]]
    source = "assets/.*\\.(js|ts|jsx|tsx)"
    target = "js"
  [[build.cachebusters]]
    source = "assets/.*\\.(.*)$"
    target = "$1"
```

Update `./config/postcss.config.js`:

```js
let tailwindConfig = process.env.HUGO_FILE_TAILWIND_CONFIG_JS || './config/tailwind.config.js';
const tailwind = require('tailwindcss')(tailwindConfig);
const autoprefixer = require('autoprefixer');

module.exports = {
  // eslint-disable-next-line no-process-env
  plugins: [tailwind, ...(process.env.HUGO_ENVIRONMENT === 'production' ? [autoprefixer] : [])],
}
```

Add `./config/tailwind.config.js`:

```js
module.exports = {
  darkMode: 'class',
  content: ['./hugo_stats.json'],
  corePlugins: {
    aspectRatio: false,
  },
  plugins: [
    require('@tailwindcss/aspect-ratio'),
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
  ]
}
```

## How to use

See the Hyas documentation:

- [Tailwind CSS](https://docs.gethyas.com/guides/integrations-guide/tailwindcss/)

## Credits

This npm package is based on:

- [Hugo Basic Starter for TailwindCSS v3.x](https://github.com/bep/hugo-starter-tailwind-basic)
