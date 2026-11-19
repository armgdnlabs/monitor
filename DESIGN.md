# DESIGN.md — ARMGDN

> Hand this file to Claude (or drop it in a repo root and point `CLAUDE.md` at it) when building any ARMGDN web app, tool, plugin UI or page.
> It describes the language of the live site (armgdnlabs.github.io) and MONITOR. **When this file and an old Figma page disagree, this file wins.**

## 1. The brand in one breath
ARMGDN is one brand: **Labs** (tools + plugins) and **Sounds** (kits, samples, bundles). The motto is *The Chaos to Create.*

The look is **dark, editorial, tactile**. Huge tight grotesk type, bone-on-black, one signal red, soft glass surfaces, analog grain and dither. It should feel like a boutique audio-visual instrument maker. It is not a SaaS dashboard, and it is no longer the old brutalist/CRT terminal look.

- **Dark is the default.** Light ("bone") is a toggle, never an afterthought. Every surface must work in both.
- **Red is a signal.** Use it for dots, pips, active values, the live state and one CTA per view. Never use it for large fills or decoration.
- **Chaos lives in the content.** That means canvas, covers and dither fields. The chrome stays calm and rounded.

## 2. Figma source of truth
File **ARMGDN — Brand System**: `https://www.figma.com/design/hB1ezCMW5DEn7Fryf1E1E9`. Use the Figma MCP (`get_variable_defs`, `get_design_context`, `get_screenshot`). In URLs `:` becomes `-` (e.g. `?node-id=40-34`).

| What | Node | Notes |
|---|---|---|
| **System 2026 page** (start here) | `40:34` | Current tokens, type and components |
| Components (masters) | `40:35` | Button `40:48` (Solid/Ghost/Red), Kick `40:52`, Tag `40:55`, Theme toggle `40:65`, Slider `40:66`, Knob `40:70`, Tabs `40:75`, Glass card `40:85` |
| Specimen, dark / light | `40:91` / `40:192` | Visual reference for both modes |
| Variables | collections **Primitives**, **Color** (modes Dark/Light), **Radius**, Spacing | WEB code syntax = the CSS custom properties below |
| Text styles | Display/XL, Display/L, Heading/M, Heading/S, Body/L, Body/M, UI/Label, UI/Small, Motto/Italic, Mono/Tag, Mono/Body, Mono/Caption, Pixel/Tag | `Legacy/*` styles are retired |
| Marks: symbol, wordmark, lock-ups | page `02 — Marks` (`1:3`), masters section `2:2`, guidelines `4:4` | Export as SVG, never redraw |
| Pixel icons, dither/scanline patterns | page `04 — Graphic Kit` (`9:44`) | Icons at integer scales only |
| ~~Foundations (legacy)~~ | `1:2` | Outdated: screen modes and VT323 display type. Don't use |
| ~~Applications~~ | `9:45` | Work in progress. **Do not follow** |

## 3. Tokens
Use these names verbatim as CSS custom properties. In Tailwind apps, expose them as space-separated RGB channels so alpha works (see MONITOR `src/index.css`).

### Primitives
| Token | Hex | | Token | Hex |
|---|---|---|---|---|
| `--void` | `#0a0a0a` | | `--ash-300` | `#bdb9b2` |
| `--ink` | `#1e1e1e` | | `--bone-200` | `#ede7df` |
| `--ash-900` | `#262523` | | `--bone` | `#f7f2ec` |
| `--ash-700` | `#4a4845` | | `--signal` | `#d3202a` |
| `--ash-500` | `#85827d` | | `--signal-2` (hover/hot) | `#ff3b3b` |

### Semantic (dark default → `[data-theme="light"]`)
| Token | Dark | Light | Use |
|---|---|---|---|
| `--bg` | `#080808` | `--bone` | Page |
| `--bg-2` | `#0f0f0e` | `--bone-200` | Raised cards, panels |
| `--fg` | `--bone` | `--void` | Primary text, solid buttons |
| `--fg-2` | `--ash-300` | `--ash-700` | Body / secondary |
| `--muted` | `--ash-500` | `--ash-500` | Meta, `//` slashes |
| `--line` | `#f7f2ec17` | `#0a0a0a17` | Hairlines, card edges |
| `--line-2` | `#f7f2ec2e` | `#0a0a0a2e` | Stronger edges, ghost button |
| `--glass` | `#f7f2ec0d` | `#0a0a0a08` | Glass fill |
| `--glass-2` | `#f7f2ec17` | `#0a0a0a12` | Hover glow, toggle track |
| well | `#00000059` | `#0c0c0c0d` | Slider tracks, segmented controls, inputs |
| `--shadow` | `0 30px 60px -24px #000c` | `0 30px 60px -28px #0a0a0a55` | Floating only |

Theme switch: `data-theme` on `<html>`, persisted in `localStorage`, set by an inline head script before first paint (no flash). Colours transition `0.5s var(--ease)`.

### Shape and space
- Radius: **card 22px** (`--r`), panel 16, cover 14, field 10, **pill 999**. Buttons, tabs, toggles and chips are always pills. Nothing in the chrome is square.
- Gutter `clamp(16px, 4vw, 40px)`; content max-width 1320px; 4/8px spacing grid.

## 4. Type
| Role | Font | Spec |
|---|---|---|
| Display | **Archivo** SemiBold | `clamp(64px,14vw,230px)/0.84`, tracking −0.065em. Section `.h2`: `clamp(48px,8vw,128px)/0.88`, −0.055em |
| Display parenthetical | Archivo Medium, `--signal` | `<sup>` at 0.13em, lowercase: **`Labs.(tools)`**, **`Monitor.(fx)`** |
| UI / headings / body | **Archivo** 400–600 | UI 13px/600; panel headers 14–16px/600, −0.02em; body 15px/1.55 |
| Numbers, values, tags | **JetBrains Mono** | Tags 11px/500, UPPERCASE, +0.14em, prefixed `// ` in `--muted`. All numeric readouts are mono (no jitter) |
| Motto only | **Tinos** Italic | *The Chaos to Create.* and rare pull-quotes. Never UI |
| Experimental flags | **Silkscreen** | Tiny cover/beta tags only |

Google Fonts: `Archivo:wdth,wght@62..125,400..700`, `Tinos:ital@0;1`, `JetBrains+Mono:wght@400;500`, `Silkscreen`. Never use Inter, system-ui, VT323 or blackletter.

## 5. Components (copy these patterns)
- **Kick** `.kick`: 9px red dot + Archivo 600 13px. It is the header for sections, panels (`.slash` in MONITOR) and modals.
- **Tag** `.tag`: `// LABEL` mono, as above.
- **Button** `.btn`: 46px pill, padding `0 18px 0 22px`, gap 12, label + 8px red **pip**. Variants: solid (fg bg / bg text), `.ghost` (glass + inset 1px `--line-2` + blur 18px), `.red` (signal bg, bone pip). Hover: the label **rolls** up (two stacked spans, `translateY(-105%)`, 0.5s spring) and the pip scales 2.4×. Small/icon buttons are round 32–36px.
- **Glass card** `.glass` / `.card`: radius 22, `--glass` fill, `inset 0 0 0 1px var(--line), inset 0 1px 0 var(--line-2)`, `backdrop-filter: blur(26px)`. `.card` adds a cursor-following radial glow (`--gx/--gy`, 420px, `--glass-2`).
- **Theme toggle**: pill track (`--glass-2`, 1px `--line-2`); the knob is a **red ring** in both modes.
- **Segmented tabs**: pill well; the active item is a solid `--fg` pill with `--bg` text.
- **Slider**: 4px well track, red value fill, round `--fg` thumb (14px), mono value readout. Drag, wheel and arrow keys all work; Shift gives fine control; double-click resets.
- **Knob**: 270° grey arc track, 3px red value arc, dot indicator. Drag vertically.
- **Fields / menus**: field radius 10, well fill, 1px line; menus are glass cards with 6px item radius.
- **Modal**: glass card at 94% panel opacity, blur 30, kick title, round close button, Esc/backdrop closes.
- **Nav**: floating glass pill (Labs · Sounds · About) + theme toggle. The A symbol always links home.
- **Ticker**: Archivo 600 13px items separated by 6px red dots; it speeds up with scroll velocity.
- **Covers** (generated art for products): dithered 2-radial gradient per hue + 4px checker multiply + scanlines, Archivo name in `mix-blend-mode: difference`, Silkscreen tag.

## 6. Motion
- Eases: `--ease cubic-bezier(.22,.61,.36,1)` (default), `--spring cubic-bezier(.2,.9,.25,1)` (hover/press), `--expo cubic-bezier(.76,0,.24,1)` (reveals, wipes).
- Reveals: `[data-reveal]` fades and rises 28px over 0.9s. Headline `.lines` slide up per line over 1.1s expo, staggered 70ms.
- Loader: once per visit (session flag). The symbol stroke draws, a `000→100` mono counter runs, a red 1px bar fills, then a clip-path wipe up. It is skipped on later navigations.
- Cursor (fine pointers only): 6px red dot + 38px ring that grows on links and fills with a label (`data-cursor="…"`).
- Logo: lens/distortion **on hover only**. No idle animation on marks.
- Smooth scroll: Lenis. Page transitions: Astro view transitions with the persisted header.
- Always honour `prefers-reduced-motion`: kill animation and show the end state.

## 7. Texture
- **Grain**: animated film grain at opacity ≈0.07 dark (screen) / 0.05 light (multiply). On the site it is a canvas tile redrawn every other frame (`src/scripts/site.ts`); MONITOR uses an SVG `feTurbulence` overlay. Canvas grain does not show in Firefox (fingerprinting protection), which is accepted.
- **Dither field**: WebGL2 ordered-dither background (`noise` and `cone` modes) behind heroes, in muted ash with occasional red. Keep it slow and low contrast.
- The app background may carry one faint signal-red radial glow top-left (≈8%).

## 8. Plugins and native UIs (JUCE, VST/AU, desktop)
Use the same tokens, rendered natively:
- Background `--bg`, panels `--bg-2` with 1px `--line`, radius 16–22. Fake the glass with a flat 4–6% bone fill (no blur needed).
- Knobs follow the Figma knob (`40:70`): grey 270° track, red value arc, dot. Value text in JetBrains Mono under the knob, label in Archivo 600.
- Header: the A symbol at top-left (from Marks), the product as `Name.(type)` with a red parenthetical, and a dark/light toggle if the host allows.
- Meters: thin rounded bars, ash with a red peak/clip. Bypass, power and record states are the only red fills.
- Bundle fonts (all OFL): Archivo, JetBrains Mono, Tinos.

## 9. Do / Don't
**Do:** use big tight grotesk headlines, generous space, pills and 22px cards, hairlines at 9% alpha, mono numbers, one red thing per view, real dark *and* light, hover-driven delight, and keyboard support.

**Don't:** use square brutalist boxes, CRT/LCD "screen modes", VT323 or blackletter, Inter/system fonts, purple/blue accents, gradients in the chrome (outside covers and fields), heavy drop shadows on resting cards, emoji, generic icon sets (use the pixel icons or simple 1.5px strokes), red as decoration, or idle-animated logos. Never distort or recolour the A apart from its circle.

## 10. Prompt to give Claude
> Build this UI following `DESIGN.md` exactly. Pull tokens with the Figma MCP from file `hB1ezCMW5DEn7Fryf1E1E9`, page **00 — System 2026** (`40:34`): variables in the Color (Dark/Light) and Radius collections, components in `40:35`, specimens `40:91` (dark) and `40:192` (light). Export marks from page `1:3` as SVG and don't redraw them. Ignore the legacy Foundations (`1:2`) and Applications (`9:45`) pages. Dark by default with a light toggle (red-ring knob). Build the shell first, screenshot it in both themes, and compare against the specimens before adding features.

## 11. MONITOR specifics
- Wordmark `Monitor.(fx)` (`.wordmark`) opens About; the A symbol links to the ARMGDN home page; "by ARMGDN" sits bottom-left.
- Tokens live in `src/index.css` as RGB channels; panels are `.glass-panel` (radius 16) with `.ph` red-dot headers. Use the existing classes (`.btn`, `.btn-primary`, `.btn-signal`, `.icon-btn`, `.tab`, `.seg`, `.field`, `.menu`, `.kbd`, `.tag*`, `.chip`, `.theme-toggle`) before inventing new ones.
- The canvas is the only thing that glows. Effects and presets mirror effect.app (see `src/effects/`).
- Marks are in `src/brand/Marks.tsx`, pixel icons in `src/brand/icons.ts`, both exported verbatim from Figma.
