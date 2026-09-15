# AGENTS.md

## Project

- This is a Craft CMS 4 site running PHP 8.3 in DDEV, with Twig templates, Tailwind CSS 3, and Alpine.js 3.
- The web root is `web/`. Templates live in `templates/`, source CSS in `src/css/`, and Craft configuration in `config/`.
- Keep solutions simple and consistent with the existing codebase. Prefer native Craft, Twig, Alpine, and Tailwind features over new abstractions or dependencies.

## Working Style

- Be concise, friendly, and human. Lead with the result and mention important tradeoffs or assumptions.
- Preserve unrelated work and follow existing naming, formatting, and template patterns.
- Keep comments brief and useful; do not explain obvious code.
- Never commit secrets or environment-specific values. Update `.env.example` with placeholders when a new variable is required.

## Development

- Use DDEV for PHP, Craft, Composer, and database commands.
- Start the site with `ddev start`.
- Run Craft commands with `ddev craft <command>` and Composer commands with `ddev composer <command>`.
- Install front-end dependencies with `npm install` when needed.
- Run `npm run watch` during front-end development and `npm run build` for a minified production stylesheet.
- Treat `web/css/styles.css` as generated output; make CSS changes in `src/css/tailwind.css` or in Twig with utility classes.

## Craft and Twig

- Use Craft element queries and Twig filters directly where they keep the template clear.
- Escape untrusted output by default. Use `|raw` only for trusted or intentionally rendered HTML.
- Avoid database schema or content-model changes unless the task requires them.
- Make control-panel changes locally, commit the resulting `config/project/` changes, and apply project config in deployed environments.
- Reuse installed plugins where appropriate, especially SEOmatic, ImageOptimize, Typogrify, Retcon, Quick Filters, CKEditor, Redactor, and the Contact Form plugin. Do not add a package when an installed plugin or Craft itself already solves the problem.

## Tailwind and Alpine

- Prefer inline Tailwind utility classes in Twig over custom CSS. Add custom CSS only for genuinely reusable or unsupported behavior.
- Match existing responsive and visual conventions before introducing new design tokens.
- Use Alpine for small, local interactions. Keep state close to its markup and favor `x-data`, `x-show`, `x-bind`, and `x-on` over separate JavaScript files.
- Preserve keyboard access, visible focus states, semantic HTML, and appropriate ARIA attributes when adding interactions.
- Alpine is currently loaded from the shared head partial; do not load it again in individual templates.

## Verification

- Run `npm run build` after changing Tailwind classes or CSS.
- Run `ddev composer validate` after changing `composer.json`.
- Run `ddev craft project-config/apply` when relevant, then check the affected pages in the browser.
- Inspect Craft logs in `storage/logs/` when diagnosing server-side failures.
- Test the smallest relevant surface first, then check nearby responsive, empty, and error states.

## Deployment

- Keep deployments compatible with `.deploy.sh`; do not replace or run the production deployment flow unless explicitly asked.
- The existing release sequence installs production Composer dependencies, runs Craft upgrades and migrations, clears caches, and drains the queue.
- Build production CSS with `npm run build` before release when front-end files change, and ensure the generated stylesheet is included in the release.
- Commit Craft project-config changes and any required migrations. Do not depend on manual production control-panel changes.
- Before deployment, confirm required environment variables, database backups, writable storage paths, and queue behavior for the target environment.
