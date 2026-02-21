# Optional Skills

These skills are domain-specific and not included in the core workflow. Copy them into your `.claude/skills/` directory if they're relevant to your project.

## Available Skills

### `design-audit/`

AI-powered UI audit skill based on proven UX principles. Evaluates interfaces against heuristics for visual hierarchy, accessibility, cognitive load, navigation, and more. Based on *Making UX Decisions* by Tommy Geoco.

**Use if:** Your project has a user-facing frontend/UI.

### `react-best-practices/`

React and Next.js performance optimization guidelines from Vercel Engineering. Covers rendering, re-renders, bundle optimization, async patterns, and more.

**Use if:** Your project uses React or Next.js.

## Installation

Copy the skill directory into your project's `.claude/skills/` folder:

```bash
# Install design-audit
cp -r optional-skills/design-audit .claude/skills/design-audit

# Install react-best-practices
cp -r optional-skills/react-best-practices .claude/skills/react-best-practices
```

After copying, the skills will be automatically available in your next Claude Code session.
