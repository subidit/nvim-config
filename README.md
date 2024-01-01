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