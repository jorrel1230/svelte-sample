# Minimal landing page

A SvelteKit site statically deployed to GitHub Pages.

- `npm install` installs dependencies.
- `npm run dev` starts the local development server.
- `npm run build` builds the static site into `build/`.

The homepage is in `src/routes/+page.svelte`, the shared header and footer are in `src/routes/+layout.svelte`, and styles are in `src/app.css`. Add another page at `src/routes/<name>/+page.svelte`. The custom domain is in `static/CNAME`.
