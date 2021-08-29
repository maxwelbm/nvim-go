# nvim-go

[![build](https://github.com/crispgm/nvim-go/actions/workflows/ci.yml/badge.svg)](https://github.com/crispgm/nvim-go/actions/workflows/ci.yml)

A minimal implementation of Golang development plugin written in Lua for neovim 0.5+.

Neovim 0.5 embeds a built-in Language Server Protocol (LSP) client so that
we are able to do most of vim-go's features by LSP client.
`nvim-go` collaborates with it and provides additional features,
and leverages community toolchains to get Golang development done.

## Features

- Auto format with `:GoFormat` when saving.
- Run linters with `:GoLint` (via `revive`) automatically.
- Quickly test with `:GoTest`, `:GoTestFunc`, `:GoTestFile` and `:GoTestAll`.
- Import packages with `:GoGet` and `:GoImport`.
- Modify struct tags with `:GoAddTags`, `:GoRemoveTags`, `:GoClearTags`, `:GoAddTagOptions`, `:GoRemoveTagOptions` and `:GoClearTagOptions`.
- Generates JSON models with `:GoQuickType` (via `quicktype`).

### Recommended Language Features

Language server provides useful language features to make Golang development easy.
`nvim-go` does not provide these features but collaborates with them.

This section can be considered as a guide or common practice to develop with `nvim-go` and `gopls`:

- Code Completion via `nvim-cmp` or other completion engine you like
- Declaration: `vim.lsp.buf.declaration()`
- Definition: `vim.lsp.buf.definition()` and `vim.lsp.buf.type_definition()`
- Implementation: `vim.lsp.buf.implementation()`
- Hover: `vim.lsp.buf.hover()`
- Signature: `vim.lsp.buf.signature_help()`
- References: `vim.lsp.buf.reference()`
- Symbols: `vim.lsp.buf.document_symbol()` and `vim.lsp.buf.workspace_symbol()`
- Rename: `vim.lsp.buf.rename()`

### Other Recommends

- Debug: we recommend [vim-delve](https://github.com/sebdah/vim-delve) or [nvim-dap](https://github.com/mfussenegger/nvim-dap).

## Installation

Prerequisites:

- Neovim (>= 0.5)
- [yarn](http://yarnpkg.com) or [npm]() (for quicktype)

Install with vim-plug or any package manager:

```viml
" dependencies
Plug 'nvim-lua/plenary.nvim'
Plug 'nvim-lua/popup.nvim'
" nvim-go
Plug 'crispgm/nvim-go'
" (recommend) LSP config
Plug 'neovim/nvim-lspconfig'
```

Run:

```shell
nvim +PlugInstall
```

Finally, run `:GoInstallBinaries` after plugin installed.

Install `quicktype` by npm:

```lua
require('go').config.update_tool('quicktype', function(tool)
    tool.pkg_mgr = 'npm'
end)
```

## Usage

### Setup

```lua
-- setup nvim-go
require('go').setup({})

-- setup lsp client
require('lspconfig').gopls.setup({})
```

### Defaults

```lua
require('go').setup{
    -- auto commands
    auto_format = true,
    auto_lint = true,
    -- linters: revive, errcheck, staticcheck, golangci-lint
    linter = 'revive',
    -- lint_prompt_style: qf (quickfix), vt (virtual text)
    lint_prompt_style = 'qf',
    -- formatter: goimports, gofmt, gofumpt
    formatter = 'goimports',
    -- test flags: -count=1 will disable cache
    test_flags = {'-v'},
    test_timeout = '30s',
    test_env = {},
    -- show test result with popup window
    test_popup = true,
    test_popup_width = 80,
    test_popup_height = 10,
    -- test open
    test_open_cmd = 'edit',
    -- struct tags
    tags_name = 'json',
    tags_options = {'json=omitempty'},
    tags_transform = 'snakecase',
    tags_flags = {'-skip-unexported'},
    -- quick type
    quick_type_flags = {'--just-types'},
}
```

### Manual

Display within neovim with:

```vim
:help nvim-go
```

## License

[MIT](/LICENSE)
