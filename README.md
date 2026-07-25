# token-system

A Claude Code skill for setting up design token systems.

For engineers who build their own products and want them to look like their own products. It sets up, migrates to, or translates a complete token system (colors, typography, spacing, radii, shadows, motion, grid and component states) and ships it as stack-specific code: CSS variables, a Tailwind v4 `@theme` block, or Kotlin objects.

It is a side effect of real work, not theory. Every product I build gets its own design system, across different stacks and different looks. What carries over from one to the next is never the code. It is the method, so I distilled it into a skill.

The reasoning behind it is written up here: [Design tokens are context for your coding agent](https://miikka.space/writing/tokens-are-context).

## Install

```
npx skills@latest add MiikkasGIT/token-system
```

Or manually:

```
mkdir -p ~/.claude/skills/token-system
curl -o ~/.claude/skills/token-system/SKILL.md https://raw.githubusercontent.com/MiikkasGIT/token-system/main/SKILL.md
```

## Why use it?

### Agents don't have a system

Left alone, an agent invents a new gray on every prompt. One small site of mine had quietly accumulated sixteen distinct hex values, and four of them were nearly identical light fills. Nothing was broken. It just was not a system.

A token file is context for your coding agent. Define `--subtext` and the agent uses it everywhere helper text appears. Define nothing and you get hex value number seventeen.

### It asks before it assumes

The skill runs its gate questions before writing a single token: platform, dark or light as brand or preference, stack posture. It proposes strong defaults and asks where a decision belongs to you instead of guessing: brand hue, web font, icon library, component layer. And it names everything for what it is for, never for what it looks like.

It configures a system. It does not design a brand and it does not build a component library. It stops where components begin: tokens ready for whatever component layer comes next.
