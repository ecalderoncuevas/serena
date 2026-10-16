# Serena — contexto para Claude Code

@AGENTS.md

Proyecto final de DAW 2 de Edu (entrega: mayo de 2027). **Serena** es una web app con un mapa interactivo de las zonas de montaña donde, **dentro de la ley**, se puede hacer vivac, pernoctar o acampar. Empieza por el Pirineo catalán y está pensada para ampliarse al resto de Cataluña y de España.

## Al empezar cada sesión

1. Lee **`docs/HISTORY.md`** → sección *Estado actual*: fase, en qué se está trabajando y cuál es el siguiente paso.
2. Si necesitas el plan completo (fases, MVP, stack, decisiones), lee **`docs/ROADMAP.md`**.
3. Comprueba la rama actual con `git branch --show-current`. Si es `main`, **para**. Si es `testing` y la tarea es de código, propón crear una rama de tarea desde `testing`.

## Al terminar una tarea o una sesión

Propón a Edu la entrada para `docs/HISTORY.md`: actualizar *Estado actual* y añadir arriba del *Registro* una entrada con fecha, ordenador, lo hecho, las decisiones y el siguiente paso. Va como último commit de la rama de la tarea.

## Cómo trabajar con Edu

- Responde en **castellano** (o en catalán si Edu te escribe en catalán).
- Edu quiere **entender el código antes de escribirlo**: explica primero qué vas a hacer y por qué, y después hazlo.
- Entrega **archivos completos**, no fragmentos sueltos.
- Si una decisión cambia el plan (stack, estructura, alcance), dilo explícitamente para que quede en `docs/HISTORY.md`, en `docs/ROADMAP.md` y en Notion.

## Reglas de Git

- **Edu hace todas las operaciones de Git desde el panel Source Control de VS Code.** No hagas `git commit`, `push`, `merge` ni `rebase`, ni crees o borres ramas, salvo que te lo pida.
- `main` (producción) está **protegida en GitHub**: solo se entra por pull request desde `testing`. Nunca se trabaja directamente en ella.
- `testing` (preview) **no está protegida** (decisión consultada con el profesor). Aun así, el código nuevo va en ramas de tarea; los cambios pequeños, como la documentación, pueden ir directos a `testing`.
- Ramas de tarea desde `testing`: `feature/…`, `fix/…` o `chore/…`, en kebab-case y sin acentos.
- Propón los mensajes de commit **en catalán** con prefijo: `feat(header): afegeix menú responsive`, `fix(mapa): corregeix el zoom en mòbil`, `docs: actualitza history`.
- Flujo: rama de tarea → PR a `testing` (*Squash and merge*, borrar la rama) → al final del sprint, PR de `testing` a `main` (*merge commit*, nunca squash).

## Stack y entorno

- **Node 24** y **npm 11**. Usa siempre **npm**, nunca pnpm, yarn ni bun.
- **Next.js 16** (App Router, Turbopack, `proxy.ts` en lugar de `middleware.ts`, `params` y `cookies()` asíncronos) + **React 19** + **TypeScript**.
- **Tailwind CSS v4** (tokens con `@theme`, colores en OKLCH).
- **shadcn/ui con Base UI** (preset `b0`). Añade componentes con `npx shadcn@latest add <componente>` y sigue la variante de **Base UI**, no la de Radix.
- **Previsto**, solo cuando la fase lo necesite y en su propia rama:
  - GSAP + `@gsap/react` y Lenis.
  - **MapLibre GL JS** + `react-map-gl/maplibre`, con cartografía de Open ICGC.
  - Supabase (PostGIS + Auth), next-intl (ca/es/en), Serwist (PWA), Zod, Vercel AI SDK.
  - Vitest + Playwright.
- Deploy en Vercel: producción desde `main`, preview desde `testing`.

## Reglas del proyecto

- **El mapa se hace con MapLibre**, no con Three.js, Babylon ni Unity. Three.js y R3F, solo para efectos de la landing.
- **Rendimiento:**
  - El mapa solo se carga en `/mapa` (import dinámico con `ssr: false`).
  - Lenis está desactivado en `/mapa`.
  - Las animaciones no pueden retrasar el LCP.
- **Estructura:** sin carpeta `src/`. Rutas en `app/[locale]/(marketing)/…` (landing, zonas y guías, con SSR/SSG) y `app/[locale]/(app)/…` (mapa y cuenta). Componentes en `components/`, hooks en `hooks/` y utilidades en `lib/`.
- **Datos legales:** nunca inventes el estado legal de una zona. Cada estado necesita una fuente oficial y una fecha de verificación.
- **Accesibilidad:** WCAG 2.2 AA. Los colores de dificultad y de estado legal van siempre acompañados de icono o texto. Respeta `prefers-reduced-motion`.
- **Mobile-first**, a ancho completo.

## Comandos

```bash
npm run dev      # http://localhost:3000
npm run build    # tiene que pasar antes de cada PR
npm run lint
npx tsc --noEmit
```

## Definición de terminado

La tarea está terminada cuando se cumple todo esto:

- Funciona en escritorio y en móvil.
- No hay errores de TypeScript ni de lint.
- `npm run build` pasa.
- Está revisada en la preview de `testing`.
- `docs/HISTORY.md` está actualizado.