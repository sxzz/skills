# Skills Generator

Generate [Agent Skills](https://agentskills.io/home) from project documentation.

PLEASE STRICTLY FOLLOW THE BEST PRACTICES FOR SKILL: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices

- Focus on agents capabilities and practical usage patterns.
- Ignore user-facing guides, introductions, get-started, install guides, etc.
- Ignore content that LLM agents already confident about in their training data.
- Make the skill as concise as possible, avoid creating too many references.

## Repository Structure

```
.
├── skills/                     # Output directory
│   └── {skill-name}/
│       ├── SKILL.md            # Skill index file
│       ├── GENERATION.md       # Tracking metadata (for generated skills)
│       └── references/
│           └── *.md            # Individual skill reference files
│
└── AGENTS.md                   # This file
```

## Workflows

### General Instructions for Generation

- Focus on agents capabilities and practical usage patterns. For user-facing guides, introductions, get-started, or common knowledge that LLM agents already know, you can skip those content.
- Categorize each reference into `core`, `features`, `best-practices`, `advanced`, etc categories, and prefix the reference file name with the category. Feel free to create more categories if needed to better organize the content.

### Creating New Skills

- **Understand** the documentation thoroughly
- **Create** skill files in `skills/{project}/references/`
- **Create** `SKILL.md` index listing all skills
- **Create** `GENERATION.md` with source information

### Updating Existing Skills

1. **Check** what has changed in the source documentation
2. **Update** affected skill files based on changes
3. **Update** `SKILL.md` with the new version of the tool/project and skills table
4. **Update** `GENERATION.md` with new source info

## File Formats

### `SKILL.md`

Index file listing all skills with brief descriptions. Name should be in `kebab-case`.

The version should be the date of the last update.

```markdown
---
name: { name }
description: { description }
metadata:
  author: Kevin Deng
  version: '2026.1.1'
  source: Generated from {source-url}, scripts located at https://github.com/sxzz/skills
---

> The skill is based on {project} v{version}, generated at {date}.

// Some concise summary/context/introduction of the project

## Core References

| Topic           | Description                                       | Reference                                        |
| --------------- | ------------------------------------------------- | ------------------------------------------------ |
| Markdown Syntax | Slide separators, frontmatter, notes, code blocks | [core-syntax](references/core-syntax.md)         |
| Animations      | v-click, v-clicks, motion, transitions            | [core-animations](references/core-animations.md) |

## Features

### Feature a

| Topic             | Description              | Reference                                |
| ----------------- | ------------------------ | ---------------------------------------- |
| Feature A Editor  | Description of feature a | [feature-a](references/feature-a-foo.md) |
| Feature A Preview | Description of feature b | [feature-b](references/feature-a-bar.md) |

// ...
```

### `GENERATION.md`

Tracking metadata for generated skills:

```markdown
# Generation Info

- **Source:** {source-url or description}
- **Generated:** 2024-01-15
```

### `references/*.md`

Individual skill files. One concept per file.

At the end of the file, include the reference links to the source documentation.

```markdown
---
name: { name }
description: { description }
---

# {Concept Name}

Brief description of what this skill covers.

## Usage

Code examples and practical patterns.

## Key Points

- Important detail 1
- Important detail 2

<!--
Source references:
- {source-url}
- {source-url}
-->
```

## Writing Guidelines

1. **Rewrite for agents** - Don't copy docs verbatim; synthesize for LLM consumption
2. **Be practical** - Focus on usage patterns and code examples
3. **Be concise** - Remove fluff, keep essential information
4. **One concept per file** - Split large topics into separate skill files
5. **Include code** - Always provide working code examples
6. **Explain why** - Not just how to use, but when and why
