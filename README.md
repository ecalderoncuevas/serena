# Serena

Web app con un mapa interactivo de las zonas de montaña donde, **dentro de la ley**, se puede hacer vivac, pernoctar o acampar. Empieza por el Pirineo catalán.

Proyecto final de DAW 2 · Edu Calderón · 2026–2027

- **Qué hay que hacer:** [`docs/ROADMAP.md`](docs/ROADMAP.md)
- **Qué se ha hecho y estado actual:** [`docs/HISTORY.md`](docs/HISTORY.md)
- **Reglas para Claude Code:** [`CLAUDE.md`](CLAUDE.md)

## Stack

Next.js 16 · React 19 · TypeScript · Tailwind CSS v4 · shadcn/ui (Base UI) · Vercel

## Requisitos

- **Node 24 o superior** (`"engines": { "node": ">=24" }` en `package.json`). Vercel compila con la 24.x; en local también sirve la 26. **npm 11**, que viene con Node.
- **Git**, y **GitHub CLI** para iniciar sesión.
- **VS Code** con la extensión *GitHub Pull Requests*.

## Empezar en un ordenador nuevo

Solo la primera vez en cada ordenador.

**1. Instalar Node (24 o superior)**

Si el ordenador ya tiene Node 24 o 26 (`node -v`), salta este paso.

- **Mac:**
  ```bash
  brew install node@24
  brew link --overwrite --force node@24
  ```
- **Windows:** descarga el instalador **LTS** desde [nodejs.org](https://nodejs.org). La 24 y la 26 sirven.

**2. Iniciar sesión en GitHub** con la cuenta `ecalderoncuevas`:

```bash
gh auth login
gh auth setup-git
```

**3. Clonar el proyecto fuera de iCloud y OneDrive**

- **Mac:**
  ```bash
  mkdir -p ~/Developer && cd ~/Developer
  ```
- **Windows (PowerShell):**
  ```powershell
  mkdir $HOME\Developer; cd $HOME\Developer
  ```

Después, en los dos:

```bash
git clone https://github.com/ecalderoncuevas/serena.git
cd serena
git switch testing
npm install
npm run dev
```

Abre <http://localhost:3000>.

**4. Variables de entorno:** cuando el proyecto las tenga, copia `.env.example` a `.env.local` y rellena las claves. **Nunca subas `.env.local` a GitHub.**

## Cada vez que te sientas a trabajar

1. En VS Code: cámbiate a `testing` y haz **Sync**.
2. Si ha cambiado `package-lock.json`, ejecuta `npm install`.
3. Lee **Estado actual** en [`docs/HISTORY.md`](docs/HISTORY.md).
4. Crea la rama de la tarea **desde `testing`**: `feature/…`, `fix/…` o `chore/…`.
5. Al terminar, añade tu entrada en `HISTORY.md`, haz commit y abre el PR hacia `testing`.

## Flujo Git

- `main` es producción y está protegida: **solo se entra por pull request desde `testing`**.
- `testing` es la preview y no está protegida. El código nuevo va en ramas de tarea; los cambios pequeños (docs) pueden ir directos.
- Rama de tarea → PR a `testing` → **Squash and merge** → borrar la rama.
- Al final de cada sprint: PR de `testing` a `main` → **Create a merge commit**.
- Commits en catalán: `feat(header): afegeix menú responsive`.

## Comandos

```bash
npm run dev           # desarrollo
npm run build         # build de producción
npm run lint          # ESLint (no funciona por ahora: ver HISTORY.md)
npm run typecheck     # errores de TypeScript
npm run format        # formatea el código con Prettier
```