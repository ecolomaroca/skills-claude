# skills-claude

Colección de skills para Claude Code (y otros agentes compatibles con `.agents/skills`), agrupadas en tres plugins.

## Estructura

```
.agents/skills/<skill>/        fuente única de cada skill (SKILL.md + scripts/ + references/ …)
.claude/skills/<skill>  ->     symlinks a .agents/skills para que Claude Code las cargue en este repo
.claude-plugin/marketplace.json   define los plugins instalables
.claude/agents/<agente>.md     subagentes de Claude Code (uno por plugin)
LICENSES/                      licencias de los proyectos de origen
```

Reglas para añadir algo nuevo:

- **Skill nueva:** crea `.agents/skills/<nombre>/SKILL.md` con `name` igual al nombre de la carpeta, crea el symlink `ln -s ../../.agents/skills/<nombre> .claude/skills/<nombre>` y añade la ruta al plugin que corresponda en `marketplace.json`.
- **Scripts** en `scripts/`, documentación larga en `references/` (mantén `SKILL.md` por debajo de ~500 líneas).
- **Subagente nuevo:** `.claude/agents/<nombre>.md` con frontmatter `name`, `description` y opcionalmente `tools` y `model`.
- Si la skill viene de otro repo, añade su licencia a `LICENSES/` y una fila en la tabla de abajo.

## Instalación

En cualquier proyecto, desde Claude Code:

```
/plugin marketplace add ecolomaroca/skills-claude
/plugin install design@skills-claude
/plugin install instagram@skills-claude
/plugin install design-imagegen@skills-claude   # opcional
```

Dentro de este repo las skills de `.claude/skills/` se cargan solas.

## Agentes

| Agente | Plugin | Skills precargadas | Cuándo lo usa Claude |
|---|---|---|---|
| `taste-designer` | `design` | `design-taste-frontend`, `redesign-existing-projects` (las de estilo las lee solo si se piden) | Crear, rediseñar o revisar webs públicas |
| `instagram-strategist` | `instagram` | las 13 `ig-*` | Cualquier tarea de contenido o crecimiento en Instagram |

Claude los elige solo según la tarea, o puedes pedirlo explícitamente: "usa el agente instagram-strategist para planificar mi semana". Las skills listadas en `skills:` se cargan enteras en el contexto del agente al arrancar.

## Skills

Las marcadas como **manual** tienen `disable-model-invocation: true`: Claude no las activa solo, se llaman con `/nombre`. Así no compiten entre sí cuando pides "diséñame una web".

### Plugin `design`

| Skill | Uso | Origen |
|---|---|---|
| `design-taste-frontend` | Dirección de arte para webs nuevas (por defecto). Requiere Node 22+ y Chrome para sus scripts | taste-skill |
| `redesign-existing-projects` | Mejorar una web o app existente | taste-skill |
| `high-end-visual-design` | Estilo "agencia premium" — **manual** | taste-skill |
| `minimalist-ui` | Estilo editorial minimalista — **manual** | taste-skill |
| `industrial-brutalist-ui` | Estilo brutalista/industrial — **manual** | taste-skill |
| `gpt-taste` | Variante con GSAP y layouts aleatorios — **manual** | taste-skill |
| `full-output-enforcement` | Obliga a generar código completo, sin placeholders — **manual** | taste-skill |

### Plugin `design-imagegen`

Necesitan una herramienta de generación de imágenes, o están escritas para otro agente. No tienen symlink en `.claude/skills/`.

| Skill | Uso | Origen |
|---|---|---|
| `brandkit` | Tableros de identidad de marca (imagen) | taste-skill |
| `image-to-code` | Genera imágenes de diseño y luego las implementa (escrita para Codex) | taste-skill |
| `imagegen-frontend-web` | Referencias visuales de webs, una imagen por sección | taste-skill |
| `imagegen-frontend-mobile` | Referencias visuales de pantallas móviles | taste-skill |
| `stitch-design-taste` | Genera `DESIGN.md` para Google Stitch | taste-skill |

### Plugin `instagram`

Comparten estado en `~/.claude/instagram/` (`voice.md`, `swipe.md`, `plan.md`, `log.md`). Todo texto pasa por `/ig-human` antes de mostrarse. Los scripts usan Python 3 sin dependencias.

| Skill | Uso |
|---|---|
| `ig-plan` | Plan semanal de contenido y engagement |
| `ig-viral` | Busca reels que funcionan en tu nicho y crea un swipe file |
| `ig-reel` | Guion de reel: hooks, texto en pantalla, beat sheet |
| `ig-carousel` | Carrusel slide a slide |
| `ig-caption` | Caption, hashtags y lint |
| `ig-story` | Secuencia de stories |
| `ig-repurpose` | Convierte un contenido largo en reels y carruseles |
| `ig-profile` | Puntúa y reescribe el perfil |
| `ig-audit` | Analiza qué ha funcionado en tus posts |
| `ig-comment` | Comentarios en posts ajenos |
| `ig-reply` | Respuestas a comentarios propios |
| `ig-dm` | DMs de outreach y seguimiento |
| `ig-human` | Quita rasgos de texto generado por IA |

Origen: instagram-agent-skill.

## Créditos y licencias

| Proyecto | Autor | Licencia |
|---|---|---|
| [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) | Leonxlnx | MIT — `LICENSES/taste-skill.MIT.txt` |
| [Jakeschincariol/instagram-agent-skill](https://github.com/Jakeschincariol/instagram-agent-skill) | Jake Schincariol | MIT — `LICENSES/instagram-agent-skill.MIT.txt` |

Cambios respecto al original: se quitaron `design-taste-frontend-v1` y `design-taste-frontend-v2-experimental` (siguen en el historial de git y en el repo de origen), los scripts de las `ig-*` se movieron a `scripts/`, y algunas skills de estilo pasaron a invocación manual.

En Windows, activa `git config core.symlinks true` antes de clonar para que funcionen los enlaces de `.claude/skills/`.
