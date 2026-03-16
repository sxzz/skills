---
name: sxzz
description: Kevin Deng (@sxzz)'s opinionated tooling and conventions for TypeScript projects. Use when setting up new projects, configuring ESLint/Prettier, or when the user mentions sxzz's preferences.
metadata:
  author: Kevin Deng
  version: '2026.03.16'
---

## Coding Practices

### Code Organization

- **Single responsibility**: Each source file should have a clear, focused scope/purpose
- **Split large files**: Break files when they become large or handle too many concerns
- **Co-locate types**: Keep types and interfaces in the same file where they are used to preserve context
- **Co-locate constants**: Keep constants in the same file where they are used. Only extract to a dedicated file when shared across multiple files

### Runtime Environment

- **Prefer isomorphic code**: Write runtime-agnostic code that works in Node, browser, and workers whenever possible

### TypeScript

- **Explicit return types**: Declare return types explicitly when possible
- **Avoid complex inline types**: Extract complex types into dedicated `type` or `interface` declarations

### Comments

- **Avoid unnecessary comments**: Code should be self-explanatory
- **Explain "why" not "how"**: Comments should describe the reasoning or intent, not what the code does

### Testing (Vitest)

- Test files: `foo.ts` → `foo.test.ts` (same directory)
- Use `describe`/`it` API (not `test`)
- Use `toMatchSnapshot` for complex outputs
- Use `toMatchFileSnapshot` with explicit path for language-specific snapshots

---

## Tooling Choices

### @antfu/ni Commands

| Command                    | Description                                |
| -------------------------- | ------------------------------------------ |
| `ni`                       | Install dependencies                       |
| `ni <pkg>` / `ni -D <pkg>` | Add dependency / dev dependency            |
| `nr <script>`              | Run script                                 |
| `nu`                       | Upgrade dependencies                       |
| `nun <pkg>`                | Uninstall dependency                       |
| `nci`                      | Clean install (`pnpm i --frozen-lockfile`) |
| `nlx <pkg>`                | Execute package (`npx`)                    |

### ESLint + Prettier Setup

ESLint with `@sxzz/eslint-config`:

```js
// eslint.config.js
// @ts-check
import { sxzz } from '@sxzz/eslint-config'

export default sxzz()
```

Prettier with `@sxzz/prettier-config`:

```json
{
  "prettier": "@sxzz/prettier-config"
}
```

When completing tasks, run `pnpm run lint --fix` to fix coding style, then `pnpm run format` to format with Prettier.

---

## References

| Topic               | Description                                         | Reference                                                |
| ------------------- | --------------------------------------------------- | -------------------------------------------------------- |
| Project Setup       | .gitignore, GitHub Actions workflows                | [setting-up](references/setting-up.md)                   |
| Library Development | tsdown bundling with tsdown-preset-sxzz, publishing | [library-development](references/library-development.md) |
