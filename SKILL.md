---
name: token-system
description: >
  Set up, migrate to, or translate a complete design token system (colors,
  typography, spacing, radii, shadows, motion, grid and component states),
  delivered as stack-specific code (CSS vars, Tailwind @theme, or Kotlin
  objects) plus a living styleguide page rendered from the tokens. Use when
  starting a new project, when hardcoded values are scattering across an
  existing codebase, or when a Figma design system needs to become code.
  Opinionated defaults, but asks where the decision belongs to the user.
  It configures a system; it does not design a brand and it does not build a component library. It stops where components begin: tokens ready for whatever component layer comes next.
---

# Token System

## Operating Posture

You are a design engineer whose defining trait is **clarity**.

The premise of this skill: a design system is not just team discipline anymore.
It is **context for AI-assisted development**. Every token you define is an
instruction a coding agent will follow; the clearer the system, the faster and
better every future session gets. That is why everything here is named for
**what it is for, never for what it looks like**.

Two failure modes, equally bad. **Dogma**: designing the system as if it were
law. It is a guideline: a generalization that will meet edge cases it cannot
cover. That is expected, not a failure. **Genericness**: accepting framework
defaults wholesale and shipping something that looks like everything else.

One asymmetry to hold on to: **the system is a guideline for the human and a
law for you.** You follow "never" better than "usually", so apply the rules
without exceptions and write the token file to state its rules without
exceptions. Breaking out is a human decision: when reality breaks the system,
propose the local exception with a comment and ask; never break out silently.

Therefore: propose strong defaults and commit to them. Where a decision belongs
to the user (icon library, component kit), **ask, don't assume**. The second
time the same exception appears, it becomes a token.

## The Gate

Four questions, in order, before any token is written:

0. **Starting point?** Blank project → Workflow A: Define. Existing codebase
   with scattered values → Workflow B: Migrate. Design system already in
   Figma → Workflow C: Translate.
1. **Platform?** Mobile app, web app, or both. Typography branches on this:
   platform font and larger bodies on mobile, chosen font and reading
   line-heights on web.
2. **Mode: brand or preference?** For social and consumer products, dark or
   light can be part of the brand. Then pick one and commit, no toggle. For
   SaaS it is a user preference: build light first, derive dark with judgment.
3. **Stack posture?** Blank slate (Compose, plain CSS): the risk is never
   building the system, so build up. Batteries included (Tailwind): the risk
   is shipping the defaults, so narrow down. Same destination either way:
   one small set of roles.

## Ask, don't assume

Decisions that belong to the user. Suggest a default, then ask:

- **Icon library** (suggest Lucide; custom icons if branding demands it)
- **Component layer** (own kit, shadcn/Radix, or none yet). Not to build it,
  but to verify token coverage against it, and to map token names onto an
  existing contract (e.g. shadcn's CSS variables) instead of inventing a
  parallel vocabulary
- **Brand hue(s)** (the one color that is truly theirs)
- **Web font** (mobile gets the platform font; web is a choice)
- **Styleguide page** (default yes on new projects; ask on established ones)

## Domain Knowledge

Reference values come from three shipped systems: a dark-first native app
(Compose), a light-first SaaS (Tailwind/CSS vars), and a typographic site.

### Colors

Roles, never values. A finished set covers: Backing (page), Module/Surface
(cards), Highlight (borders/faint fills), Subtext, Maintext, one Brand hue
(+ light companion), and status (Error, Success, Warn).

- **Text hierarchy is alpha, not new grays.** One ink color, stepped by opacity.
  (SaaS: `--ink: #212121` with 13 alpha steps; dark app: white at
  0.02–0.80.) One source of truth, automatic harmony.
- **Lines and fills get separate names even at identical alphas.** A border and
  a background are different intentions; `bg-[var(--line-subtle)]` reads wrong
  even when the value is right. And in dark mode they diverge: lines go
  *fainter* on dark (a hairline at 0.08 on near-black reads as a hard edge).
- **A color earns a ramp only if it is interactive** (brand, error → hover/
  pressed steps). Static roles stay single values.
- Neutral ladder: ~5 visible steps is enough (e.g. #FAFAFA → #EDEDF0 →
  #E4E5E9 → #8F98A0 → #1C1D1F).
- Dark mode: a second value set with judgment, never naive inversion.

### Typography

Roles named for use (Title, Body, Bold, Small, Button Text), sized per platform:

- **Mobile:** platform font (SF Pro / Roboto). Ladder 32/24/20/18/16/14,
  three weights (Medium, Semibold, Bold; no Regular, body is Medium).
- **Web:** one chosen font. Smaller body (14–16px), real reading line-height
  (1.6–1.7). Dense steps at reading sizes, big jumps on top. No modular-scale math.

### Spacing

Numeric ladder on a 4px base (2–64) **plus a few named slots** for recurring
places: `screen` (edge padding), `section` (between major blocks), `bottom`
(CTA / safe area). Slots are where consistency actually breaks. Name them.

### Radii

3–5 fixed steps, project-wide. Derive from usage, not theory: count what the
code uses, keep steps that are visibly apart, fold 2px neighbours into each
other (5→4, 9→8, 18/20→16). A 10-step scale in a real audit collapsed exactly
this way.

### Shadows

3–4 elevation steps (resting, raised, floating, overlay). Defined once,
never composed ad hoc.

### Motion

One standard ease-out (`cubic-bezier(0.23, 1, 0.32, 1)`), optionally one
overshoot (`--ease-back`) for playful moments, and 2–3 durations. Enough for
95% of UI.

### Grid & Breakpoints

Max content width + column count as tokens. Breakpoints: take the framework's
unless the design demands otherwise (that is a narrow-down decision, not a
blank-slate one).

### Component coverage

This skill does not build components. Components consume tokens; most projects
bring their own layer (shadcn, Radix, an in-house kit). The four states
(**hover/pressed, disabled, loading, error/validation**) become a coverage
test instead: the system is complete when a Button, a Field and a Modal could
be built from tokens alone. Walk the planned kit (Button, Field, Link,
Checkbox, Card, Badge, Modal, Toast, Dropdown) and check: interactive ramps,
disabled alphas, validation colors, overlay shadow. Verify, don't implement.

### Migration heuristics (Workflow B)

- Scan for literals (hex, px, font sizes). Cluster near-duplicates.
- **A value appearing twice or more is a token candidate** (same rule as
  exception promotion).
- Real "before" case: a small site accumulated 16 distinct hex values; four
  were near-identical light fills (#f0f0f0–#f5f5f5). They fold to ~7 roles.

## Workflow A: Define (blank project)

Always in this order, colors first; everything else leans on them:

1. **Colors.** Ask for the brand hue (Ask, don't assume). Build the role set
   with the neutral ladder, ink-alpha text hierarchy, and ramps only for
   interactive colors. Every token gets a usage line.
2. **Typography.** Platform branch from the Gate. Roles, weights, ladder.
3. **Spacing & Radii.** 4px ladder + named slots; 3–5 radii.
4. **Shadows, Motion, Grid.** Elevation steps, one ease-out (+ optional
   overshoot), max-width + columns.
5. **Component coverage check.** Ask which component layer is planned (Ask,
   don't assume), then walk the kit checklist and verify the tokens can
   express every state. Add missing tokens; do not build components.
6. **Generate the styleguide page** (see Required Output) and wire tokens
   into the stack (blank slate: define; Tailwind: replace defaults in
   `@theme` and delete what you don't use).

## Workflow B: Migrate (existing codebase)

1. **Scan** for literals: hex/rgb/hsl, px sizes, font sizes, radii, shadows.
   Report counts per value; the numbers drive everything after.
2. **Cluster** near-duplicates: values within ~2px (radii/spacing) or barely
   distinguishable shades fold together. Show the fold table before acting.
3. **Map** clusters onto the role model by *where they are used*, not what
   they look like: the value on page backgrounds becomes Backing, the value
   on borders becomes a line token.
4. **Replace call sites** incrementally, per role, verifying visually per
   batch. Never one giant find-and-replace.
5. **Leftovers** (used once, no role fits) stay local exceptions with a
   comment. They are the promotion watchlist, not a failure.

## Workflow C: Translate (Figma exists)

1. **Read** variables and styles via the official Figma MCP
   (`get_variable_defs`); fall back to asking for a style export.
2. **Rename** what-it-looks-like names into what-it-is-for roles
   (`Light Grey` → `Module`). Keep the original name in the usage line so
   designers can still find things.
3. **Flag the gaps** Figma systems typically leave: interaction states,
   motion, breakpoints, loading/error states. Fill them with the Domain
   Knowledge defaults and mark them for review.
4. **Generate** stack-specific code and the styleguide page. The Figma file
   stays the design source; the code becomes the build source of truth.

## Required Output

One artifact is non-negotiable, one is offered:

1. **Token code, stack-specific** (required). CSS custom properties, a
   Tailwind v4 `@theme` block, or a Kotlin `object`, matching the project.
   Every token carries its usage as a comment. No orphan tokens: if nothing
   uses it and nothing is planned to, it does not ship. This file, with its
   usage comments, is what a coding agent actually reads: the token source
   itself is the AI context, not any rendered page.
2. **A living styleguide page** (`/design` route or equivalent, offered).
   Rendered *from the tokens themselves*, never a separate document. It
   serves humans: new teammates, designers, stakeholders. Generate it by
   default on new projects (Workflow A). On an established codebase
   (Workflows B and C), offer it and let the user decide; a team of one on
   a mature project rarely needs it.

Close with a short decision log: what was chosen, what was asked (Ask,
don't assume answers), and the exception watchlist.
