# typescript-native-lsp

A Claude Code plugin that runs TypeScript 7's native (Go) language server. Use it in place of the official `typescript-lsp` plugin, which still uses `typescript-language-server`.

The server starts through `pnpx`, so nothing needs to be installed globally:

```
pnpx --silent --package=typescript@7 tsc --lsp --stdio
```

`typescript@7` resolves to the latest stable 7.x release. Prereleases (the `7.1.0-dev` builds that npm's `latest` tag currently points at) are excluded. pnpm caches the package, so only the first start downloads it.

It always uses this TypeScript 7 build, not the project's own `typescript` dependency. Projects on TypeScript 5 or 6 have a `tsc` without `--lsp`.

## Requirements

- `pnpm` on your `PATH`

## Install

```sh
claude plugin marketplace add ~/code/personal/claude-typescript-lsp
claude plugin install typescript-native-lsp@claude-typescript-lsp
claude plugin disable typescript-lsp@claude-plugins-official
```

Disable the official plugin so two TypeScript servers aren't attached to the same files.

To try it without installing:

```sh
claude --plugin-dir ~/code/personal/claude-typescript-lsp
```

## Limitations

- **No per-file diagnostics.** tsgo only publishes project-level (tsconfig) diagnostics on its own; per-file errors are pull-only (`textDocument/diagnostic`). Claude Code doesn't pull them, so type errors don't show up automatically after edits. Navigation (hover, definition, references, symbols, call hierarchy) works.
- Vue, Svelte, Astro, MDX and Angular templates aren't covered. They depend on `tsserver` plugins, which TypeScript 7 doesn't load.
