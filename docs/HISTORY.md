# Serena — Historial del proyecto

Qué se ha hecho, qué se ha decidido y dónde estamos. Lo que queda por hacer está en [`ROADMAP.md`](ROADMAP.md).

**Cómo se usa**

- **Al empezar a trabajar** (en cualquier ordenador): haz *Sync* de `testing` y lee **Estado actual**.
- **Al terminar la sesión:** actualiza **Estado actual** y añade una entrada arriba del todo en **Registro**. Hazlo en la rama de la tarea en la que estés, como último commit (`docs: actualitza history`), para que entre en el mismo PR.

---

## Estado actual

*Última actualización: 16/10/2026 · Mac*

- **Fase:** F0 · Descubrimiento y setup. Prevista hasta el 11/10, se está alargando para cerrar la investigación.
- **Sprint:** S1 · Setup e investigación.
- **Hecho recientemente:** `main` protegida con un ruleset; documentación del proyecto en el repo.
- **En curso:** entrevistas a usuarios y MoSCoW del MVP.
- **Siguiente paso:** pedir la clave de Meteocat y análisis de la competencia.
- **Bloqueos:** esperando respuesta para las entrevistas.

---

## Registro

*La entrada más reciente va arriba. Plantilla al final del archivo.*

### 16/10/2026 · Mac

- **Hecho:**
  - Rama `main` protegida con un ruleset en GitHub (PR obligatorio, sin borrado ni force push).
  - `CLAUDE.md`, `README.md`, `docs/ROADMAP.md` y `docs/HISTORY.md` añadidos al repo.
  - Base de datos *Funcionalidades (MoSCoW)* en Notion con 32 funcionalidades y una propuesta de prioridad.
  - Guion de entrevistas separado para influencers y para amigos y conocidos (Notion › Investigación).
- **Decisiones:**
  - `testing` sin proteger, consultado con el profesor. Solo `main` exige PR.
  - Squash hacia `testing` y merge commit hacia `main`.
  - Sin carpeta `src/`: rutas en `app/` en la raíz, como dejó la plantilla.
  - Documentación del repo dividida en `CLAUDE.md`, `README.md`, `docs/ROADMAP.md` (qué hacer) y `docs/HISTORY.md` (qué se ha hecho).
  - En el MoSCoW, *cuentas de usuario* baja a Should, la ruta offline se divide en GPX (Must) y PWA (Should) y *qué llevar* entra como Should. Pendiente de validar con las entrevistas.
- **Siguiente paso:** clave de Meteocat, competencia.

### 05/10/2026 · Mac

- **Hecho:**
  - Project brief en Canva (4 diapositivas con diagramas) para la entrega 01.
  - Espacio en Notion para el profesorado: bases de datos de Proyectos (fases con cronograma) y Tareas.
- **Decisiones:** el espacio para profesores es una copia; se actualiza una vez por sprint.

### 01/10/2026 · Mac

- **Hecho:**
  - Repositorio conectado a Vercel.
  - Notion de trabajo montado: Roadmap, Sprints, Tareas, Decisiones, Bugs, Diario, Investigación, Diseño, SEO, y Memoria y defensa.
- **Ideas:** información de fauna y flora de cada zona (añadida como Could).

### 25/09/2026 · Mac

- **Hecho:**
  - Entorno: Node 24 con Homebrew (sin fnm), npm 11, GitHub CLI (`gh`) y el comando `code`.
  - Proyecto creado en `~/Developer/serena`, fuera de iCloud, con `npx shadcn@latest init --preset b0 --template next --name serena`.
  - Ramas `main` y `testing` publicadas en GitHub (`ecalderoncuevas/serena`).
- **Decisiones:**
  - Node 24 porque Vercel aún no admite Node 26 en las builds.
  - shadcn/ui con Base UI (preset b0).
  - Siempre npm.

### 24/09/2026 · Arranque

- **Hecho:** idea definida, roadmap de 7 fases hasta mayo de 2027 y análisis de 8 webs de inspiración.
- **Decisiones:**
  - Nombre: **Serena** (de *dormir a la serena*).
  - Una sola web (landing + app) en lugar de dos.
  - MapLibre GL JS para el mapa, en lugar de Three.js, Babylon o Unity.
  - El estado legal de cada zona siempre con fuente oficial y fecha de verificación.
  - Flujo Git: ramas de tarea → `testing` → `main`.

---

## Plantilla de entrada

```markdown
### DD/MM/AAAA · Mac / PC casa / Instituto

- **Hecho:**
- **Decisiones:**
- **Problemas:**
- **Siguiente paso:**
```