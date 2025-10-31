# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

Project overview
- Framework: SvelteKit 2 (Svelte 5, TypeScript)
- Build tool: Vite 7
- Styling: Tailwind CSS v4 (via @tailwindcss/vite)
- Lint/format: ESLint + Prettier (with prettier-plugin-svelte)
- Adapter: adapter-auto

Commands
- Install deps
  ```sh path=null start=null
  npm install
  ```
- Dev server (open browser)
  ```sh path=null start=null
  npm run dev -- --open
  ```
- Build and preview production
  ```sh path=null start=null
  npm run build
  npm run preview
  ```
- Typecheck (svelte-check) and watch
  ```sh path=null start=null
  npm run check
  npm run check:watch
  ```
- Lint and format
  ```sh path=null start=null
  npm run lint
  npm run format
  ```
- Regenerate SvelteKit type bindings (when updating routes, env, etc.)
  ```sh path=null start=null
  npm run prepare   # runs: svelte-kit sync
  ```
- Tests
  - No test runner is configured in this repo.

Architecture and structure
- Routes/layout
  - src/routes/+layout.svelte sets global head (favicon from $lib/assets), imports global styles (src/app.css), and renders child routes.
  - src/routes/+page.svelte composes page UI from local components (Header, NewCollection).
- Components
  - src/components/Header.svelte
    - Responsive header with desktop nav, mobile slide-in menu, and icon actions.
    - Uses Svelte transitions (fade, fly), guards DOM access for SSR, and manages body scroll lock when mobile menu is open.
    - Composes SearchBar.svelte.
  - src/components/SearchBar.svelte
    - Simple, typed search input with inline SVG icon.
  - src/components/NewCollection.svelte
    - Embla carousel integration (embla-carousel-svelte) with an init hook exposing the API; styles and example slides included.
- Libraries and aliases
  - $lib points to src/lib (standard SvelteKit alias). Place shared modules/assets under src/lib (e.g., src/lib/assets/...).
- Styling
  - Tailwind CSS v4 via @tailwindcss/vite in vite.config.ts; no separate tailwind.config is required for basic usage.
  - Global stylesheet is imported in +layout.svelte (src/app.css).
- Tooling/config
  - vite.config.ts registers tailwindcss() and sveltekit() plugins.
  - svelte.config.js uses vitePreprocess() and adapter-auto.
  - tsconfig.json extends the generated .svelte-kit/tsconfig.json and enables strict, checkJs, source maps, bundler module resolution.
  - .prettierrc configures Prettier (tabs, singleQuote, printWidth, svelte plugin and parser override).

Notes
- The README includes the basic dev/build/preview commands; the scripts above mirror and extend those with lint/format/typecheck.
