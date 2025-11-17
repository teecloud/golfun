## golfun — Copilot / AI Agent Guidance

This document captures discoverable, actionable knowledge for AI coding agents working in this Ionic + Angular (standalone) repository.

Quick facts
- Project: Ionic + Angular (v20) with Capacitor.
- Scripts (repo root `package.json`):
  - `npm start` -> `ng serve` (development)
  - `npm run build` -> `ng build` (output configured to `www/`)
  - `npm test` -> `ng test` (Karma)
  - `npm run lint` -> `ng lint` (ESLint via angular-eslint)

Architecture & routing
- `src/main.ts`: app bootstrapped with `bootstrapApplication`. Provides Ionic and Router providers and registers `IonicRouteStrategy`.
- `src/app/app.routes.ts`: top-level routes. Root route lazy-loads `./tabs/tabs.routes`.
- `src/app/tabs/tabs.routes.ts`: tabbed layout and the primary example of lazy-loading pages using `loadComponent` (e.g. `import('../tab1/tab1.page').then(m => m.Tab1Page)`).
- Pattern: top-level uses `loadChildren` to load route bundles; tabs use `loadComponent` for each page.

Project conventions and patterns to follow
- Standalone components: pages/components are standalone and declare their own `imports` in `@Component`. Do not add NgModule files.
- Styling: SCSS only. Global styles found in `src/global.scss` and theming in `src/theme/variables.scss`.
- Prefix: Angular prefix is `app`. Keep generated selectors consistent (e.g., `app-root`).
- Lazy-loading: Prefer `loadComponent` for page routes under `tabs/` and `loadChildren` for larger route groups (see `app.routes.ts` and `tabs.routes.ts`).

How to add a page or route (example)
- Add new standalone page under `src/app/my-page/`.
- In `src/app/tabs/tabs.routes.ts` add a child route with `loadComponent: () => import('../my-page/my-page.page').then(m => m.MyPage)`.
- Keep redirects consistent: root redirects to `/tabs/tab1`.

Testing, linting, and CI
- Unit tests: `src/**/*.spec.ts` run with Karma (`npm test`). See `karma.conf.js` and `tsconfig.spec.json`.
- Lint: `ng lint` with angular-eslint (patterns set in `angular.json`).

Capacitor / native notes
- Capacitor configs exist (`capacitor.config.ts`). After adding native plugins, run `npx cap sync` and rebuild the native project.

Files to inspect when debugging common issues
- Routing errors: `src/main.ts`, `src/app/app.routes.ts`, `src/app/tabs/tabs.routes.ts`.
- Build issues: `angular.json`, `tsconfig.app.json`, `package.json` devDependencies.
- Styling issues: `src/global.scss`, `src/theme/variables.scss`, and component SCSS files.

Examples of concrete code patterns
- Route preloading is enabled in `main.ts` with `withPreloading(PreloadAllModules)`.
- Ionic route strategy provided: `{ provide: RouteReuseStrategy, useClass: IonicRouteStrategy }` in `main.ts`.

Notes for the agent when editing
- Preserve standalone component approach — do not convert pages to NgModules.
- When changing routes, update both the route file and any unit tests that import the routes.
- Use existing folder layout under `src/app/` and mirror naming conventions (e.g., `tab1`, `tab2`).

If any of this guidance is incomplete or you want examples expanded, tell me which area (routing, building, tests, Capacitor) to expand.
