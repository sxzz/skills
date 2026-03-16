---
name: library-development
description: Building and publishing TypeScript libraries with tsdown and tsdown-preset-sxzz. Use when creating npm packages, configuring library bundling, or setting up package.json exports.
---

# Library Development

| Aspect       | Choice                                    |
| ------------ | ----------------------------------------- |
| Bundler      | tsdown with `tsdown-preset-sxzz` preset   |
| Output       | Pure ESM only (no CJS)                    |
| DTS          | Generated via tsdown                      |
| Exports      | Auto-generated via tsdown                 |
| publint/attw | Auto-checked via tsdown (CI-only)         |
| Inline deps  | Explicitly bundled via `inlineDeps` field |

## TypeScript Config

```json
{
  "compilerOptions": {
    "target": "esnext",
    "lib": ["es2023"],
    "moduleDetection": "force",
    "module": "nodenext",
    "moduleResolution": "nodenext",
    "resolveJsonModule": true,
    "types": ["node"],
    "allowImportingTsExtensions": true,
    "strict": true,
    "noUnusedLocals": true,
    "declaration": true,
    "noEmit": true,
    "esModuleInterop": true,
    "isolatedDeclarations": true,
    "isolatedModules": true,
    "verbatimModuleSyntax": true,
    "erasableSyntaxOnly": true,
    "skipLibCheck": true
  },
  "include": ["src", "tests"]
}
```

## tsdown Configuration

Use `tsdown-preset-sxzz` for a pre-configured setup:

```ts
// tsdown.config.ts
import { lib } from 'tsdown-preset-sxzz'

export default lib()
```

The `lib()` preset includes:

- ESM format, platform `neutral`
- DTS generation
- Auto `exports` field management
- publint + attw checks (CI-only)

For Node-specific libraries, use `nodeLib()`:

```ts
import { nodeLib } from 'tsdown-preset-sxzz'

export default nodeLib()
```

### Entry Point Options

The `entry` option supports shorthand values:

| Value       | Resolves to    |
| ----------- | -------------- |
| `'index'`   | `src/index.ts` |
| `'shallow'` | `src/*.ts`     |
| `'all'`     | `src/**/*.ts`  |

```ts
export default lib({ entry: 'shallow' })
```

Or pass a custom entry:

```ts
export default lib({
  entry: ['src/index.ts', 'src/utils.ts'],
})
```

### Inline Dependencies

Use `inlineDeps` to explicitly bundle specific dependencies into the output:

```ts
export default lib({
  inlineDeps: ['some-small-util', /^@internal\//],
})
```

This maps to tsdown's `deps.onlyBundle` option.

## package.json

Required fields for a pure ESM library:

```json
{
  "type": "module",
  "exports": {
    ".": "./dist/index.js",
    "./package.json": "./package.json"
  },
  "types": "./dist/index.d.ts",
  "files": ["dist"],
  "publishConfig": {
    "access": "public"
  },
  "engines": {
    "node": ">=20.19.0"
  },
  "scripts": {
    "build": "tsdown",
    "dev": "tsdown --watch",
    "test": "vitest",
    "release": "bumpp",
    "prepublishOnly": "pnpm run build"
  }
}
```

- **`engines.node`**: Set to the minimum LTS Node.js version
- **`exports`**: Managed by tsdown when the preset enables `exports: true`
- **`prepublishOnly`**: Ensures the package is built before publishing, preventing stale artifacts
