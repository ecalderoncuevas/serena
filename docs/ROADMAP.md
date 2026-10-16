# Serena — Roadmap del proyecto final DAW 2

*Septiembre 2026 → mayo 2027 · **Qué hay que hacer.** Lo que ya se ha hecho y el estado actual están en [`HISTORY.md`](HISTORY.md). Cómo arrancar en un ordenador nuevo: [`README.md`](../README.md).*

## 0. Resumen en 30 segundos

- **Una sola web, no dos.** Un único proyecto Next.js con dos "zonas": la parte pública ligera (landing + páginas de cada zona, pensadas para SEO) y la app pesada (`/mapa`), que solo carga el mapa cuando entras en ella. Nada de bloquear el móvil: en la montaña, el móvil es justamente el dispositivo que usarán.
- **Cambio importante de stack:** el mapa no se hace con Three.js, Babylon ni Unity, sino con **MapLibre GL JS** (mapa real, relieve 3D, gratis, va bien en móvil). Three.js se queda para efectos de la landing.
- **Tu mayor valor diferencial es la ley:** una capa del mapa que diga *dónde se puede pernoctar, en qué condiciones y según qué fuente oficial*. Eso no lo ofrece bien casi nadie y es lo que Google premiará.
- **Nombre: Serena** (de la expresión catalana *dormir a la serena*: dormir al aire libre, de noche). Repositorio: `serena`.
- **Roadmap en 7 fases** con *feature freeze* el **26 de abril de 2027** y colchones en Navidad y Semana Santa.

---

## 1. ¿Landing y app separadas o todo en una misma web?

**Mi recomendación: una sola web (un solo repositorio, un solo dominio), con la landing y la app bien separadas por dentro.**

Por qué no hacer dos apps separadas:

1. **SEO.** La autoridad de un dominio se reparte si separas en dos webs o dominios. Te interesa que todo sume en el mismo sitio: landing, páginas de zonas, guías y app.
2. **Doble mantenimiento.** Dos proyectos significa dos despliegues, dos sistemas de login, dos sets de dependencias y el doble de ramas. Para un proyecto de una sola persona en 8 meses es demasiado.
3. **El problema de rendimiento no se arregla separando, sino cargando bien.** En Next.js puedes hacer que el mapa (lo pesado) solo se descargue en la ruta `/mapa`. La landing y las páginas de zona siguen siendo rapidísimas.

Cómo se organiza dentro del mismo proyecto (App Router, *route groups*):

```
app/[locale]/
  (marketing)/          ← ligero, SSR/SSG, SEO
    page.tsx            → landing
    zones/[slug]/       → una página por zona (SEO)
    guies/[slug]/       → guías y artículos
    preus/              → free vs premium
  (app)/                ← pesado, cliente
    mapa/               → el "arsenal": mapa interactivo
    compte/             → rutas guardadas, perfil
```

**Sobre el aviso de "mejor en portátil o tablet":** en lugar de un aviso que bloquee, te propongo convertirlo en una **ventaja por dispositivo** (tu punto 4):

| | Escritorio / tablet — *Planifica a casa* | Móvil — *Segueix a la muntanya* |
|---|---|---|
| Mapa | 3D con relieve, capas, comparar zonas | 2D por defecto, 3D opcional, panel inferior deslizable |
| Rutas | Perfil de desnivel grande, comparar varias | Ruta guardada offline, tu posición GPS |
| IA | Chat amplio para planificar el fin de semana | Respuestas cortas |
| Extra | Imprimir / exportar GPX | Botón "Cómo llegar" (Google Maps / Waze) |

Si quieres un aviso, que sea un *toast* discreto y cerrable ("La vista 3D completa se disfruta más en pantalla grande"), nunca un muro.

> Si en el futuro quieres de verdad `app.tudominio.com`, Next.js permite *multi-zones* sin cambiar de repositorio. No hace falta decidirlo ahora.

---

## 2. Nombre y repositorio

- **Proyecto:** Serena · **Repositorio:** `serena` (en tu cuenta principal `ecalderoncuevas`).
- Nombre decidido: **Serena**. Pendiente: comprobar el dominio (`.cat`/`.com`/`.es`) antes de comprarlo en F1.

**Aclaración sobre las ramas:** el nombre del proyecto va en el **repositorio**, no en una rama. Las ramas fijas son `main` y `testing`, y las ramas de tarea siguen tu convención (`feature/…`, `fix/…`, `chore/…`). Lo tienes detallado en el punto 10.

---

## 3. Lo más importante: "dentro de la ley"

Es tu propuesta de valor, y también tu mayor riesgo. En Cataluña **no hay una única regla**: depende del espacio natural, la altitud, el horario, el municipio y la propiedad del terreno. Algunos ejemplos reales:

- **Norma general:** la acampada se regula para campings (Decret 75/2020). Fuera de ellos, en general hace falta autorización (Decret 148/1992) y hay ordenanzas municipales.
- **Parc Natural de les Capçaleres del Ter i del Freser:** pernoctar (con o sin tienda) está permitido por encima de 2.000 m, entre las 20 h y las 8 h, una sola noche en el mismo sitio.
- **Parc Nacional d'Aigüestortes i Estany de Sant Maurici:** prohibidos la acampada y el vivac fuera de los refugios listados (y dos zonas concretas).
- **Parc Natural de l'Alt Pirineu:** autorización previa con 15 días de antelación.

**Qué implica para la app:**

1. Cada zona del mapa tendrá un **estado legal**: permitido · regulado (con condiciones) · con autorización · prohibido · sin datos. Cada uno con color, **icono** y texto.
2. Cada estado lleva **fuente oficial enlazada** y la **fecha de la última verificación**. Esto te da credibilidad y SEO.
3. **Aviso legal** claro: la información es orientativa y no sustituye a la normativa oficial.
4. Capas estacionales: en verano, el riesgo de incendio (Pla Alfa) y la prohibición de fuego; en invierno, el riesgo de aludes (butlletí de l'ICGC).
5. La IA **nunca decide sola si algo es legal**: lee el estado de tu base de datos y cita la fuente.

Referencia útil: **Montaña Regulada** (proyecto sin ánimo de lucro con un mapa de zonas reguladas de España). Úsalo para contrastar, no como fuente oficial. También puede ser un buen contacto para tu proyecto.

---

## 4. Revisión de tus funcionalidades (con mejoras realistas)

| Tu idea | Qué mantener | Qué ajustar |
|---|---|---|
| Descubrir la zona de forma realista | Mapa 3D con relieve + ortofoto ICGC | Animaciones de cámara *fly-to* al entrar en una zona |
| El tiempo de la semana "100% accurate" | Previsión de 7 días | Ninguna previsión es 100% fiable. Muestra la fuente, la hora de actualización y la previsión **a la altitud del punto** |
| Ruta sin cobertura | Mejor offline que imprimir | **PWA offline** + **exportar GPX** (lo abren Wikiloc, Komoot o un reloj GPS). El GPS funciona sin datos |
| Dificultad con colores de pistas de esquí | Verde / azul / rojo / negro | Añadir **forma o icono** (daltonismo: rojo y verde se confunden) y datos objetivos: distancia, desnivel +, tiempo estimado (inspirado en el método **MIDE**) |
| Cómo llegar (Google Maps / Waze) | Sí | Con *deep links* es gratis y no necesita API key: `google.com/maps/dir/?api=1&destination=LAT,LNG` y `waze.com/ul?ll=LAT,LNG&navigate=yes` |
| Guardar rutas y reservas | Guardar rutas y favoritos (con límite en el plan free) | Las **reservas reales** requieren acuerdos con cada negocio. Para el TFG: enlaces a los proveedores o una "solicitud de reserva" simulada |
| Free / premium | Diseñar los límites desde el principio | Cobro con Stripe en **modo test**, al final y como opcional |
| Agente IA | Fase 5 | Responde con tus datos (RAG) y **cita fuentes**. Límite de mensajes en el plan free (coste) |

**Idea extra para vivac: "modo noche".** Interfaz en tonos rojos oscuros que no deslumbra y conserva la visión nocturna. Queda muy bien en una defensa y tiene sentido real.

---

## 5. Stack: revisión y recomendación

| Capa | Recomendado | Notas |
|---|---|---|
| Framework | **Next.js 16** (App Router) + **React 19** + **TypeScript** | En npm, el 24/09/2026: next 16.3.x. Turbopack por defecto, `proxy.ts` sustituye a `middleware.ts` |
| Estilos | **Tailwind CSS v4** | Colores en OKLCH y *design tokens* con `@theme` |
| Componentes | **shadcn/ui** (base) + **React Bits** (solo detalles vistosos) | Ojo: es *React Bits*, no "Bites" |
| Animación | **GSAP 3.15** + `@gsap/react` (`useGSAP`) | Desde 2025 GSAP es 100% gratis, incluidos SplitText, ScrollTrigger, etc. |
| Smooth scroll | **Lenis** (paquete `lenis`) | Ojo: es *Lenis*, no "Lennis". **Desactivarlo en `/mapa`**: el mapa gestiona su propio scroll y zoom |
| **Mapa** | **MapLibre GL JS** + `react-map-gl/maplibre` | Open source, WebGL, relieve 3D, globo, funciona bien en móvil |
| 3D decorativo | **Three.js / React Three Fiber** | Solo en la landing (hero, montaña estilizada). MapLibre también permite meter modelos Three.js sobre el terreno |
| Base de datos | **Supabase** (Postgres + **PostGIS** + Auth + Storage) | PostGIS permite consultas geográficas: "zonas legales a menos de 30 km", "¿este punto está dentro de un parque?" |
| Offline / PWA | **Serwist** (`@serwist/next`) | Cachea rutas y *tiles* de una zona |
| Idiomas | **next-intl** (ca / es / en) | Suma mucho al SEO. Configúralo desde el día 1 |
| IA | **Vercel AI SDK** (`ai`) | Fase 5 |
| Validación | **Zod** | Formularios y datos de APIs |
| Tests | **Vitest** + **Playwright** | Y Lighthouse para rendimiento |
| Deploy | **Vercel** + GitHub | `main` = producción, `testing` = preview fija |

**¿Por qué no Three.js, Babylon o Unity para el mapa?** Son motores 3D, no sistemas de mapas. Tendrías que programar a mano coordenadas, zoom por niveles, carga de teselas, etiquetas, rutas… (meses de trabajo). Unity WebGL además pesa decenas de MB, va mal en móvil y Google no puede leer su contenido. MapLibre te lo da hecho y te deja centrarte en el diseño y en los datos.

---

## 6. Datos y APIs (todo con opción gratuita)

- **Mapa base de Cataluña: ICGC (Open ICGC).** Estilos vectoriales listos para MapLibre (topográfico, estándar, ortofoto, oscuro…) y **relieve 3D de 5 m** para toda Cataluña. Licencia CC BY: hay que citar al ICGC.
- **Relieve para ampliar a España:** Mapterhorn (teselas de terreno públicas y globales) e IGN/CNIG.
- **Tiempo:** **Open-Meteo** (sin clave, previsión por coordenada y altitud) + **Meteocat API** (oficial de Cataluña, incluye predicción del Pirineo; hay que pedir clave, con cuota limitada: **pídela ya**). Para España: **AEMET OpenData**.
- **Espacios naturales protegidos:** capas de ENP/PEIN en las dades obertes de la Generalitat (polígonos para tu capa legal).
- **Puntos de interés** (refugios, fuentes, aparcamientos, restaurantes): OpenStreetMap vía Overpass API, más datos curados por ti.
- **Rutas:** GeoJSON/GPX creados o curados por ti. No copies de Wikiloc, que no tiene API pública y cuyo contenido tiene derechos.

**Modelo de datos pensado para escalar a toda España:**

```
region (Catalunya) → area (Pirineu) → zone (polígono, estado legal, fuentes, verificado_en)
                                     → spot (punto: vivac, refugio, parking, fuente)
                                     → route (línea, dificultad, distancia, desnivel, tiempo)
```

URLs escalables: `/ca/catalunya/pirineu/capcaleres-ter-freser`. Añadir Aragón o Asturias será añadir filas, no reescribir código.

---

## 7. Análisis de tus 8 webs de inspiración

> **Nota de transparencia:** varias de estas webs se generan con JavaScript/WebGL y mis herramientas solo pudieron leer su estructura, sus metadatos y fichas públicas (Awwwards y similares). **Vita Travel bloquea la lectura automática** (robots.txt). Te recomiendo completar el análisis con Safari Web Inspector y la extensión **Wappalyzer**, y apuntarlo en la base de datos de inspiración de Notion.

**1. visitmeatopia.com**: universo de marca de Smithfield presentado como un **pueblo ficticio con lugares** (10 "localizaciones", cada una con su página, y 92 recetas repartidas entre ellas).
→ *Qué coger:* **el mapa como forma de navegar**, y cada lugar con **su propia URL indexable**. Es exactamente tu estrategia SEO: cada zona de montaña, una página. En la landing podrías tener un **mapa ilustrado del Pirineo** (estilizado, no funcional) que lleve a las zonas.

**2. logartis.info**: portfolio de Gergely Gizella (desarrollador e ilustrador). Mención de honor en Awwwards (2021). Tecnología: Three.js, WebGL, GLSL, GSAP y Angular. Paleta blanco y negro, scroll experimental con ilustración y **sonido**.
→ *Qué coger:* ilustración + shaders y **sonido ambiente opcional** (viento, río, grillos de noche), siempre en *mute* por defecto. *Cuidado:* es muy pesado. Solo para la landing y con alternativa si el usuario tiene activado `prefers-reduced-motion`.

**3. adaline.ai**: SaaS de IA hecho con Next.js. Estructura de manual: hero → logos → 4 funcionalidades → testimonios → proceso en 4 pasos → confianza (seguridad) → footer con **paisaje de colinas ilustrado**.
→ *Qué coger:* **la narrativa de la landing**: problema ("no sé dónde puedo dormir legalmente") → cómo funciona en 4 pasos (Explora · Comprueba · Planifica · Ve) → beneficios → CTA. Y un footer con el perfil de montañas del Pirineo.

**4. quechua-lookbook.com/ss25**: la que más encaja con tu temática. **Site of the Day en Awwwards** (1/5/2025), del estudio The MUZES para Decathlon. Paleta tierra (`#DAD6CD` / `#2A2928`), galería con scroll en perspectiva, vídeo que se revela, elementos que siguen el ratón, transiciones entre anclas, **coordenadas GPS usadas como elemento tipográfico**, 11 idiomas.
→ *Qué coger:* paleta natural y sobria, **coordenadas y altitudes como decoración tipográfica** (p. ej. `42°35′N 1°05′E · 2.143 m`), revelado de imágenes con el scroll y un menú animado.

**5. catskills-showcase.pages.dev**: tienda ficticia de arena para gatos (Vite, imágenes AVIF, `noindex`). Secciones: hero, beneficios semana a semana, comparativa, filosofía, guía paso a paso, reseñas, **3 planes de precio**, FAQ.
→ *Qué coger:* bloque **comparativa** ("Buscar en foros y Google" vs "Serena"), **tabla de planes Free/Premium**, **FAQ** (con datos estructurados FAQPage para SEO) y la idea "semana a semana" para el tiempo. *No copies el `noindex`*: tú quieres que te indexen.

**6. vita-travel.webflow.io** ("Explore by Destination"): no la he podido leer. El patrón habitual y el que te propongo para tu mapa: **lista de zonas a la izquierda + mapa a la derecha, sincronizados**. Al pasar por encima de una zona se ilumina su polígono; al hacer clic, la cámara vuela a ella (`flyTo`) y se abre el panel de detalle. En móvil, el mapa arriba y la lista en un panel inferior deslizable.

**7. bryangarage.dev/showcase**: portfolio de Bryan Oh (diseñador de producto; BMW, VW, Google). **Línea de tiempo 3D infinita** que se mueve arrastrando, con la rueda o con scroll.
→ *Sí, sirve para las estaciones:* un **carrusel circular de 12 meses o 4 estaciones**. Al girarlo cambia lo que recomienda el mapa (invierno: esquí, raquetas y aludes; otoño: setas y castañas; verano: vivac y kayak), la paleta de color y las capas (nieve, riesgo de incendio). Se implementa con GSAP (`Observer` + `Draggable` + `wrap`) o con R3F si lo quieres en 3D.

**8. 11moissanstoit.com**: diario de la vuelta al mundo de Josué y Philippe, con un **globo que carga y muestra el recorrido**.
→ *Qué coger:* **storytelling con cámara**: la ruta se va dibujando sobre el mapa (animación de línea) mientras la cámara la sigue. MapLibre tiene proyección de globo y animaciones de cámara. Ideal para la intro de la app o para presentar cada ruta. Incluye también un buen estado de carga ("Cargando el Pirineo…").

---

## 8. SEO desde el primer día

**Idea clave:** el SEO tarda entre 3 y 6 meses en dar resultados. Por eso la landing y el dominio propio tienen que estar **online antes de Navidad**, para que en mayo tengas datos reales que enseñar en la memoria. Una web en `*.vercel.app` no posicionará bien: **compra el dominio en noviembre**.

**SEO técnico (Next.js lo hace fácil):**

- Páginas de zona **pre-renderizadas** (SSG) con contenido único: estado legal, cómo llegar, mejor época, rutas, tiempo.
- `generateMetadata` (title y description únicos), `sitemap.ts`, `robots.ts`, URLs canónicas.
- **hreflang** ca/es/en con next-intl.
- **Datos estructurados JSON-LD:** `TouristAttraction` / `Place` con `GeoCoordinates`, `BreadcrumbList`, `FAQPage`, `Article`.
- Imágenes Open Graph dinámicas con `next/og` (muy útil cuando alguien comparte una zona por WhatsApp).
- **Core Web Vitals:** LCP < 2,5 s, INP < 200 ms, CLS < 0,1. El hero con GSAP/WebGL no puede retrasar el LCP: carga el texto primero y la animación después.

**Contenido (lo que de verdad posiciona):**

- *Keywords* con intención de búsqueda: "on es pot fer vivac a Catalunya", "acampada lliure Pirineu legal", "pernoctar a la muntanya normativa", "vivac [nombre de la zona]".
- Guías largas y útiles (normativa explicada, qué llevar, ética *leave no trace*) que enlacen a las páginas de zona.
- **Autoridad:** enlaces desde clubs excursionistas, foros, tu GitHub y LinkedIn, y presentar la web a Awwwards/CSSDA (también dan enlaces).
- **Buscadores con IA** (ChatGPT, Perplexity, AI Overviews): respuestas claras, FAQs, fuentes citadas, un `llms.txt` y dar de alta la web en **Bing Webmaster Tools**.

**Herramientas:** Google Search Console, Bing Webmaster Tools, PageSpeed Insights/Lighthouse, Semrush (versión gratuita limitada), Ahrefs Webmaster Tools (gratis con web verificada), Rich Results Test. *"is-agentic": no he encontrado una herramienta con ese nombre exacto. Existen similares, como agent-ready.dev, que miden si tu web la entienden los agentes de IA.*

---

## 9. Conceptos de diseño para tu Notion

Tú ya tienes: proporción áurea, "curva de Euler" y teoría del color. Sobre la curva de Euler: la **espiral de Euler (clotoide)** es la base de las **esquinas suaves tipo *squircle*** que usa Apple. En CSS hay `corner-shape: squircle` (en navegadores recientes).

**Composición y layout**

- **Escala tipográfica modular** (1,25 · 1,333 · 1,618 = áurea) y **tipografía fluida** con `clamp()`.
- **Sistema de 8 pt** para espaciados, y **grid de 12 columnas** a ancho completo con *container queries*.
- **Principios de la Gestalt** (proximidad, semejanza, cierre, figura-fondo).
- **Jerarquía visual** y patrones de lectura **F / Z**.
- **Regla de los tercios** y **espacio negativo**.

**Color**

- **Regla 60-30-10**.
- **OKLCH** (el espacio de color de Tailwind v4, con una luminosidad perceptual uniforme).
- Contraste **WCAG 2.2 AA** y **APCA**.
- Paletas **seguras para daltonismo** (clave para los colores de dificultad).
- **Modo oscuro** y **modo noche rojo**.

**Leyes de UX**

- **Fitts** (botones grandes para usarlos con guantes).
- **Hick** (pocas opciones a la vez).
- **Jakob** (el mapa debe funcionar como los mapas que el usuario ya conoce).
- **Miller**.
- **Doherty** (respuesta en menos de 400 ms).
- **Von Restorff** (lo importante destaca).
- **Divulgación progresiva**.

**Movimiento**

- Los **12 principios de la animación** de Disney.
- **Curvas de *easing*** (`cubic-bezier`, las *eases* de GSAP).
- Duraciones de 150–300 ms en la interfaz y más largas en el storytelling.
- Respetar `prefers-reduced-motion`.
- *Micro-interacciones*.

**Cartografía**

- **Jerarquía de capas** (base → relieve → zonas → rutas → puntos → etiquetas).
- **Generalización por zoom**.
- Leyenda siempre visible.

**Sistema**

- ***Design tokens*** (Figma → variables CSS).
- ***Atomic design*** (átomos → moléculas → organismos).

---

## 10. Flujo Git (tu sistema, con dos ajustes importantes)

Tu flujo está muy bien planteado. Lo formalizo:

```
main      ← producción (tu dominio)            · solo recibe desde testing
testing   ← preview fija en Vercel             · recibe las ramas de tarea
feature/header · feature/mapa-base · fix/menu-mobil · chore/actualitza-deps
```

1. **Crea cada rama de tarea desde `testing`**, no desde `main`, para que lleve lo último integrado.
2. Commits frecuentes en catalán, con prefijo: `feat(header): afegeix menú responsive` · `fix(mapa): corregeix el zoom en mòbil` · `chore(deps): actualitza next a 16.3`.
3. Terminas la tarea → *Pull Request* de la rama a `testing` → **Squash and merge** → borrar la rama.
4. Pruebas en la URL de preview de `testing` (móvil y escritorio).
5. Al final de cada sprint → *Pull Request* de `testing` a `main` → **"Create a merge commit" (NO squash)** → etiqueta de versión (`v0.1.0`…) desde GitHub Releases.

**Ajuste 1: por qué no hacer squash de `testing` a `main`.** Si aplastas `testing` en un único commit al pasarlo a `main`, las dos ramas quedan con historiales distintos. En el siguiente merge Git te enseñará conflictos "fantasma" de cosas que ya estaban subidas. El squash, solo de tarea → `testing`.

**Ajuste 2: hazlo todo desde VS Code con la extensión oficial "GitHub Pull Requests".** El merge normal de VS Code no permite squash, pero esa extensión sí: creas el PR, eliges *Squash and merge* y marcas "borrar rama", sin salir del editor.

**Protección (hecho el 16/10/2026):** `main` está protegida con un *ruleset*: PR obligatorio, sin borrado ni force push. `testing` no está protegida (decisión consultada con el profesor). Squash hacia `testing` y merge commit hacia `main`. Añade una GitHub Action que haga `lint + typecheck + build` en cada PR. Vercel, además, crea una preview por cada PR.

**Definición de "terminado"** para cada tarea: funciona en escritorio y móvil · sin errores de TypeScript ni de lint · la build de la preview pasa · revisado en `testing` · tarea actualizada en Notion.

---

## 11. Entorno de desarrollo

Las instrucciones para preparar un ordenador (Mac, PC de casa o instituto) están en el [`README.md`](../README.md). Resumen de lo decidido:

- **Node 24** y **npm 11** (siempre npm). Sin fnm. No actualizar npm a la 12.
- Carpeta del proyecto **fuera de iCloud / OneDrive**: `~/Developer/serena` (Mac) o `C:\Users\<usuario>\Developer\serena` (Windows).
- Proyecto creado con `npx shadcn@latest init --preset b0 --template next --name serena` (Next.js 16 + TypeScript + Tailwind v4 + shadcn/ui sobre Base UI).
- Las dependencias (GSAP, Lenis, MapLibre…) se instalan **cuando la fase las necesita**, cada una en su rama.

---

## 12. Roadmap: septiembre 2026 → mayo 2027

| Fase | Fechas | Objetivo | Hito |
|---|---|---|---|
| **F0** Descubrimiento y setup | 24 sep – 11 oct | Definir el MVP, investigar y preparar el entorno | Repo + Notion + MVP cerrado |
| **F1** UX/UI y datos | 12 oct – 8 nov | Diseño en Figma y primer dataset | Figma aprobado + 10 zonas |
| **F2** Base + landing + SEO | 9 nov – 20 dic | Landing en producción con dominio | **Web online e indexándose** |
| *Colchón* Navidad | 21 dic – 10 ene | Recuperar retrasos y escribir contenido | — |
| **F3** Mapa core | 11 ene – 14 feb | Mapa, zonas, estado legal, rutas, tiempo | **Demo al tutor** |
| **F4** Usuarios, offline y móvil | 15 feb – 19 mar | Cuentas, guardar rutas, PWA, GPX | Beta usable en la montaña |
| *Colchón* Semana Santa | 20 – 29 mar | Recuperar retrasos y probar en campo | — |
| **F5** IA, premium y extras | 30 mar – 25 abr | Agente IA, planes, estaciones | **Feature freeze: 26 abr** |
| **F6** Pulido y entrega | 26 abr – mayo | QA, rendimiento, memoria, defensa | Entrega final |

*Si tienes horas de formación en empresa en primavera, avísame y reajustamos F4–F5.*

### F0 · Descubrimiento y setup (24 sep – 11 oct)

- Análisis de la competencia: Wikiloc, Komoot, AllTrails, Outdooractive, Park4Night, Montaña Regulada, Refugios no guardados. ¿Qué hacen bien? ¿Qué hueco dejan libre? (El hueco: legalidad + planificación + zona concreta.)
- 5 mini entrevistas a gente que hace vivac o acampada: ¿cómo buscan información hoy?
- Investigación legal de 10–15 zonas del Pirineo con fuentes oficiales.
- Pedir la clave de la API de Meteocat. Crear las cuentas de Supabase y Vercel.
- Definir el MVP con **MoSCoW** (punto 13). Preparar Notion. Crear el repo con `main` y `testing`.
- Ramas: `chore/setup-projecte`, `chore/config-eslint-prettier`, `chore/ci-github-actions`.

### F1 · UX/UI y datos (12 oct – 8 nov)

- Personas (2–3), *user flows*, mapa del sitio y wireframes de baja fidelidad.
- **Design system en Figma:** tokens de color (OKLCH), escala tipográfica áurea, espaciado de 8 pt, iconos de dificultad y de estado legal, modo oscuro y modo noche.
- Alta fidelidad de: landing, `/mapa` (escritorio + móvil), página de zona, página de ruta y cuenta.
- Prototipo clicable y test de usabilidad con 3–5 personas.
- Esquema de la base de datos (region → area → zone → spot/route) y **primer dataset de 10 zonas** en GeoJSON.
- **Comprar el dominio** a final de fase.

### F2 · Base + landing + SEO (9 nov – 20 dic)

- Tokens en Tailwind, layouts `(marketing)` y `(app)`, i18n (ca/es), header, footer y 404.
- Landing: hero, cómo funciona, estaciones (versión simple), comparativa, planes y FAQ. Animaciones con GSAP + Lenis.
- SEO base: metadata, sitemap, robots, JSON-LD, OG images, primeras 3 guías.
- Dominio conectado en Vercel · alta en Search Console y Bing Webmaster Tools.
- Ramas: `feature/design-tokens`, `feature/header`, `feature/footer`, `feature/landing-hero`, `feature/landing-com-funciona`, `feature/i18n`, `feature/seo-base`.

### F3 · Mapa core (11 ene – 14 feb)

- MapLibre + estilo ICGC + relieve 3D + controles (con carga diferida solo en `/mapa`).
- Zonas desde Supabase/PostGIS con **capa de estado legal**, filtros por actividad y estación, y panel de detalle.
- Lista y mapa sincronizados (inspiración Vita) y *fly-to*.
- Rutas con color de dificultad y **perfil de desnivel**.
- Botones de Google Maps / Waze.
- Widget del tiempo (Open-Meteo + Meteocat).
- Páginas SSG `/zones/[slug]` generadas desde la base de datos.
- Ramas: `feature/mapa-base`, `feature/capa-legal`, `feature/panel-zona`, `feature/rutes`, `feature/perfil-desnivell`, `feature/temps`, `feature/pagines-zona`.

### F4 · Usuarios, offline y móvil (15 feb – 19 mar)

- Supabase Auth, perfil, **guardar rutas y favoritos con el límite del plan free**.
- **PWA + ruta offline** (Serwist) + exportar GPX + posición GPS.
- Adaptación a móvil: panel inferior deslizable, botones grandes, 2D por defecto.
- Carrusel de estaciones completo (inspiración Bryan Garage) y modo noche.
- Prueba real en la montaña durante la Semana Santa.

### F5 · IA, premium y extras (30 mar – 25 abr)

- Agente IA con RAG sobre tus zonas, puntos de interés y fiestas mayores; cita fuentes y tiene límite de uso.
- Puntos de interés cercanos (OSM): restaurantes, alojamientos y refugios.
- Planes Free/Premium con Stripe en modo test. Reservas simplificadas (enlace o solicitud).
- Storytelling de rutas (inspiración 11 mois).
- **26 de abril: feature freeze.** A partir de aquí no entra ninguna funcionalidad nueva.

### F6 · Pulido y entrega (26 abr – mayo)

- Tests E2E de los flujos clave. Auditoría de accesibilidad (WCAG 2.2 AA). Lighthouse ≥ 90.
- Auditoría SEO con datos reales de Search Console (¡contenido para la memoria!).
- Política de privacidad y cookies (RGPD), aviso legal y *disclaimer* de normativa.
- Memoria, vídeo demo de 2–3 minutos, presentación y ensayo de la defensa.

---

## 13. MVP con MoSCoW

> **Borrador inicial.** La propuesta actualizada (32 funcionalidades, con valor, esfuerzo y la regla del 60%) está en Notion › *Funcionalidades (MoSCoW)* y se cierra después de las entrevistas.

- **Must (imprescindible en mayo):** landing · mapa del Pirineo con 15–25 zonas · capa legal con fuentes · páginas de zona con SEO · rutas con dificultad y desnivel · tiempo · cómo llegar · cuentas + guardar rutas · ruta offline/GPX · ca/es · responsive.
- **Should (muy recomendable):** agente IA básico · carrusel de estaciones · modo noche · planes Free/Premium (simulados) · inglés.
- **Could (si sobra tiempo):** reservas · reseñas de usuarios · sonido ambiente · globo 3D en la intro.
- **Won't (este curso):** reservas reales con negocios · toda España · apps nativas.

---

## 14. Rutina para no llegar con prisas

- **Sprints de 2 semanas.** El lunes eliges las tareas del sprint en Notion. **Cada tarea de Notion = una rama.**
- **Cada día que trabajes:** como mínimo un commit y un push.
- **Viernes:** PR de las tareas terminadas a `testing` y revisión en el móvil.
- **Final de sprint:** `testing` → `main`, una *release* con etiqueta y **5 líneas en el diario** (te servirán para escribir la memoria).
- **Una vez al mes:** revisión con el tutor + repasar los riesgos + mirar Search Console.
- **Regla de oro:** si una tarea se alarga el doble de lo previsto, se divide o se baja de prioridad. No se arrastra.

---

## 15. Qué poner en Notion

Crea dentro de la página del proyecto:

1. **Home**: visión en una frase, enlaces (repo, producción, preview de `testing`, Figma, Supabase, Search Console) y estado actual.
2. **Roadmap**: base de datos de fases (fechas, estado, hito) en **vista Timeline**.
3. **Tareas / Backlog**: base de datos **Kanban** con estas propiedades:
    - *Tipo* (feature / fix / chore)
    - ***Rama*** (p. ej. `feature/header`)
    - *Fase*
    - *Sprint*
    - *Prioridad MoSCoW*
    - *Estado* (Backlog · En curso · En testing · En main)
    - *Enlace al PR*
    - *Fecha*
4. **Sprints**: una página por sprint con objetivo, tareas relacionadas y retro (qué fue bien y qué mejorar).
5. **Decisiones (ADR)**: fecha, decisión, alternativas y motivo. Ejemplos: "MapLibre en lugar de Three.js" o "una sola web". **Oro para la defensa.**
6. **Investigación**:
    - Competencia
    - **Inspiración** (una fila por web: qué cojo, captura, técnica)
    - **Normativa** (zona, estado, fuente, fecha de verificación)
    - **APIs y datos** (clave, límites, licencia)
7. **Diseño**: design system, **glosario de conceptos de diseño** (punto 9) y enlaces de Figma.
8. **SEO**: lista de keywords, checklist técnico y registro mensual de Search Console (clics, impresiones, posición).
9. **Bugs**: base de datos vinculada a las ramas `fix/…`.
10. **Diario de desarrollo**: una entrada por sprint.
11. **Memoria y defensa**: índice de la memoria, borradores y guion de la presentación.

---

## 16. Riesgos principales

| Riesgo | Probabilidad | Plan |
|---|---|---|
| Querer hacerlo todo (*scope creep*) | Alta | MoSCoW + feature freeze el 26 de abril |
| Información legal incorrecta | Media | Fuentes oficiales, fecha de verificación, *disclaimer*, empezar por pocas zonas bien hechas |
| Mapa lento en móvil | Media | 2D por defecto, carga diferida, menos capas, probar en un móvil real |
| Límites o cambios en las APIs | Media | Caché en el servidor, Open-Meteo como alternativa, pedir las claves pronto |
| Coste de la IA | Media | Límite de mensajes, respuestas cortas, fase tardía y opcional |
| Poco tráfico SEO para mayo | Alta | Dominio y landing antes de Navidad, guías de contenido, enlaces |

---

## 17. Próximas tareas — cierre de la F0

La lista viva está en Notion (Tareas → vista *Sprint 1*). Pendiente para cerrar la F0:

- [ ] Rama `chore/docs-projecte`: CLAUDE.md, README.md, docs/ROADMAP.md y docs/HISTORY.md
- [ ] Pedir la clave de la API de Meteocat
- [ ] Análisis de la competencia (Notion › Investigación › Competencia)
- [ ] Entrevistas: amigos, conocidos y, si responden, influencers (guion en Notion › Investigación)
- [ ] Normativa de 10–15 zonas del Pirineo con fuentes oficiales
- [ ] Cerrar el MVP con MoSCoW (Notion › Funcionalidades (MoSCoW)) y registrarlo en Decisiones
- [ ] Rama `chore/setup-projecte`: `engines`, Prettier y estructura de carpetas
- [ ] Rama `chore/ci-github-actions`: lint + typecheck + build en cada PR (y exigirlo en los rulesets)

---

### Fuentes

- Next.js 16: [nextjs.org/blog/next-16](https://nextjs.org/blog/next-16)
- Calendario de Node.js: [nodejsdesignpatterns.com](https://nodejsdesignpatterns.com/blog/nodejs-release-schedule-changes/) · [endoflife.date/nodejs](https://endoflife.date/nodejs)
- GSAP gratis: [webflow.com/blog/gsap-becomes-free](https://webflow.com/blog/gsap-becomes-free)
- Open ICGC (estilos y relieve para MapLibre): [openicgc.github.io](https://openicgc.github.io/)
- MapLibre 3D terrain: [maplibre.org](https://maplibre.org/maplibre-gl-js/docs/examples/3d-terrain/)
- Mapterhorn: [mapterhorn.com](https://mapterhorn.com/)
- API de Meteocat: [apidocs.meteocat.gencat.cat](https://apidocs.meteocat.gencat.cat/)
- Pernoctar en Cataluña: [refugiosnoguardados.org](https://refugiosnoguardados.org/pernocta-en-cataluna/)
- Aigüestortes: [montanaregulada.org](https://montanaregulada.org/en/area/acampada-en-p-n-de-aigueestortes-i-estany-de-sant-maurici)
- Normativa del Alt Pirineu (PDF): [parcsnaturals.gencat.cat](https://parcsnaturals.gencat.cat/web/.content/Xarxa-de-parcs/alt_pirineu/coneix-nostra-feina/centre-de-documentacio/biblioteca_digital/publicacions_divulgatives/altres/Fullet-normativa-acampada/Fullet-acampada-CATvd.pdf)
- Quechua SS25 en Awwwards: [awwwards.com/sites/quechua-2025-lookbook](https://www.awwwards.com/sites/quechua-2025-lookbook)
- Logartis en Awwwards: [awwwards.com/sites/logartis](https://www.awwwards.com/sites/logartis)
- Bryan Oh: [bryangarage.dev](https://bryangarage.dev/)
- Agent Ready: [agent-ready.dev](https://agent-ready.dev/)