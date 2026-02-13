# khathathorn-jongkohpo

Next.js project with basic CI/CD + commit quality gates.

## Local development

```bash
pnpm install
pnpm dev
```

## Pre-commit checks (Husky)

This project uses `husky` + `lint-staged`.

On every commit:

- `prettier --write` runs on staged files (auto format)
- `eslint --fix --max-warnings=0` runs on staged JS/TS files

If checks fail, commit is blocked.

## Absolute import enforcement

ESLint blocks parent-relative imports in `src/**`:

- disallowed: `../...`
- expected: `@/...`

Example:

```ts
// ✅
import Button from "@/components/Button";

// ❌
import Button from "../components/Button";
```

## CI/CD workflows

### CI (`.github/workflows/ci.yml`)

Runs on:

- Pull requests to `main`
- Pushes to non-`main` branches

Checks:

- `pnpm lint`
- `pnpm format:check`
- `pnpm build`

### CD (`.github/workflows/cd.yml`)

Runs only when a PR to `main` is merged.

- Re-runs lint/format/build checks
- Runs a deploy placeholder step

Replace the deploy placeholder with your real deployment command.

## Main branch guard

`main-guard` workflow fails if a commit on `main` is not associated with a PR.

For strict blocking of direct pushes, enable GitHub branch protection/ruleset:

1. Require pull request before merging
2. Require status checks to pass (`CI` and `Main Guard`)
3. Restrict who can push to `main` (optional but recommended)
