---
title: mason-registry
---

**mason-registry** is the [mason.nvim](https://github.com/mason-org/mason.nvim) registry for [tsvsheet](https://tsvsheet.com) tooling: add it to mason's registries and install tsvsheet's editor tooling the same way you install any language server.

- Source: [tsvsheet/mason-registry](https://github.com/tsvsheet/mason-registry)
- Language: [tsvsheet/tsvsheet](https://github.com/tsvsheet/tsvsheet)

## Use

```lua
require('mason').setup({
  registries = {
    'github:tsvsheet/mason-registry',
    'github:mason-org/mason-registry',
  },
})
```

Then `:MasonInstall tsvsheet-lsp`, or add `tsvsheet-lsp` to your ensure-installed tooling list. List the tsvsheet registry first: on a package-name collision mason uses the first registry that defines the name, which keeps these definitions authoritative for tsvsheet packages.

## Packages

- **[tsvsheet-lsp](https://github.com/tsvsheet/tsvsheet.lsp)** — the Language Server Protocol server for `.tsvt` spreadsheets (diagnostics, hover, and fill code actions from the [go-tsvsheet](https://github.com/tsvsheet/go-tsvsheet) engine). Pairs with [tsvsheet.vim](https://tsvsheet.github.io/docs.tsvsheet.vim/) in Vim/Neovim, and with any LSP client that launches `tsvsheet-lsp` from `$PATH`.

## How it publishes

Every push validates each package definition against the upstream release it pins — the pin must be the upstream's latest release, every asset must exist, and every archive must contain the binary it declares — then publishes the `registry.json.zip` + `checksums.txt` release that mason's GitHub registry source fetches. A weekly run re-validates the unchanged tree, so a new upstream release turns the registry's badge red until the pin is bumped; what mason fetches is always validated content.
