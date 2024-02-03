# Neo-vim configuration
Neo-vim configuration from scratch. 

## Installing neovim
```sh
# MacOS
brew install neovim

# Windows
winget install Neovim.Neovim
```

## init.lua
Nvim supports using `init.vim` or `init.lua` as the configuration file, but not both at the same time.
The `runtimepath` of nvim expects this file to be in `~/.config/nvim/init.lua` in Mac or Linux and for Windows in `~/AppData/Local/nvim/init.lua`.

```
set expandtab
set tabstop=2
set softtabstop=2
set shiftwidth=2
```

`:source %` source this file. This is for sourcing vimscript file.

To set vim scripts and configurations in lua file we need `meta-accessors` to expose lower level vim commands in lua runtime. `vim.cmd` function takes the vim commands and converts it for lua. Thus the previous commands should be- 

```lua
vim.cmd("set expandtab")
vim.cmd("set tabstop=2")
vim.cmd("set softtabstop=2")
vim.cmd("set shiftwidth=2")
```

## Package Manager 
1. packer.nvim
2. lazy.nvim

### Install lazy.nvim
Instructions at [GitHub](https://github.com/folke/lazy.nvim?tab=readme-ov-file#-installation).
You can add the following Lua code to your init.lua to bootstrap lazy.nvim:

```lua
-- install and/or check for lazy.nvim
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git",
    "clone",
    "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable", -- latest stable release
    lazypath,
  })
end
vim.opt.rtp:prepend(lazypath)

-- declare variables for the next command
local plugins = {}
local opts = {}

-- load lazy.nvim and key bindings
require("lazy").setup(plugins, opts)
```

## Colorscheme
Get [catppuccin](https://github.com/catppuccin/nvim).

```lua
{ "catppuccin/nvim", name = "catppuccin", priority = 1000 }

-- add it to local plugins tuple 
local plugins = {
  { "catppuccin/nvim", name = "catppuccin", priority = 1000 }
}
```
This installs the plugin but doesn't enable it.
To enable most packages we need `require` and `setup` function. The `setup` function imports all the package functionality in lua runtime for neovim to execute it. So add the following after requiring lazy-

```lua
require("lazy").setup(plugins, opts)

require("catppuccin").setup()
vim.cmd.colorscheme "catppuccin"
```

## Telescope
For fuzzy finding files and grep through project. [link](https://github.com/nvim-telescope/telescope.nvim).

Add to local plugins

```lua
local plugins = {
  { "catppuccin/nvim", name = "catppuccin", priority = 1000 },
  {
    'nvim-telescope/telescope.nvim', tag = '0.1.5',
    dependencies = { 'nvim-lua/plenary.nvim' }
  }
}
```
Now to initialize it add the following after require lazy- 
```lua
require("lazy").setup(plugins, opts)

local builtin = require("telescope.builtin")
vim.keymap.set('n', '<C-p>', builtin.find_files, {})
vim.keymap.set('n', '<leader>fg', builtin.live_grep, {})
```
`find_files` is the function withing `telescope.builtin` which is loaded by `require`. This allows us to fuzzy find files in our project.


## Treesitter
[Tool](https://github.com/nvim-treesitter/nvim-treesitter) for generating abstract syntax tree. Used for code highlighting, indenting etc. `TSUpdate` updates treesitter itself. Add to local plugin. 

```lua
local plugins = {
  { "catppuccin/nvim", name = "catppuccin", priority = 1000 },
  {
    'nvim-telescope/telescope.nvim', tag = '0.1.5',
    dependencies = { 'nvim-lua/plenary.nvim' }
  },
  {"nvim-treesitter/nvim-treesitter", build = ":TSUpdate"}
}
```
Then require `nvim-treesitter.configs` and assign it to a local variable, eg. config. then `config.setup`

```lua
local config = require("nvim-treesitter.configs")
config.setup({
  ensure_installed = {"lua", "javascript"},
  highlight = { enable = true },
  indent = { enable = true },  
})
```

# Commands
1. Record macro - `qq`, to stop `q`
2. Paste macro - `@q`
3. `:source %` source this file. 
4. `:Lazy` - lazy GUI
5. `:Telescope find_files<cr>` to see if telescope.nvim is installed correctly.
6. `Ctrl p` - find files by telescope
7. `<leader>fg` - Live Grep
8. `:TSUpdate` - update parsers 
9. `:TSInstall` - install parsers


# Packages
1. Catppuccin
2. Telescope
3. Treesitter