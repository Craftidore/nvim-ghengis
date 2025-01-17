# nvim-ghengis
Convenience file operations for neovim written in lua. 

<!--toc:start-->
- [How is this different from `vim.eunuch`?](#how-is-this-different-from-vimeunuch)
- [Installation and Setup](#installation-and-setup)
- [Available Commands](#available-commands)
- [Why that name?](#why-that-name)
<!--toc:end-->

## How is this different from `vim.eunuch`?
- Written 100% in lua.
- Uses up-to-date nvim features like `vim.ui.input` or `vim.notify`. This means you can get nicer input fields via plugins like [dressing.nvim](https://github.com/stevearc/dressing.nvim), and nicer confirmation notices with plugins like [nvim-notify](https://github.com/rcarriga/nvim-notify), if they are installed and setup.
- Some minor improvements like automatically keeping the extensions when no extension is given, or moving to the trash instead of removing files.
- Except for `trashFile` and `chmodx` only vim commands or lua os-modules are used to keep shell requirements to a minimum. 

## Installation and Setup

```lua
-- Recommended (Packer)
use {"chrisgrieser/nvim-ghengis", requires = "stevearc/dressing.nvim"}

-- if you do not care about nice input fields
use "chrisgrieser/nvim-ghengis"
```

`nvim-ghengis` (and `dressign.nvim`) require no `.setup()` function. Just create keybindings for the commands you want to use:

```lua
local ghengis = require("ghengis")
local keymap = vim.keymap.set
keymap("n", "<leader>yp", ghengis.copyFilepath)
keymap("n", "<leader>yn", ghengis.copyFilename)
keymap("n", "<leader>cx", ghengis.chmodx)
keymap("n", "<leader>rf", ghengis.renameFile)
keymap("n", "<leader>nf", ghengis.createNewFile)
keymap("n", "<leader>yf", ghengis.duplicateFile)
keymap("n", "<leader>df", function () ghengis.trashFile{trashLocation = "your/path"} end) -- ; default '~/.Trash'. Requires macOS or Linux for `mv`.
keymap("x", "<leader>x", ghengis.moveSelectionToNewFile)
```

## Available Commands
- `.copyFilepath`: Copy the absolute file path. When `clipboard='unnamed[plus]`, copies to the `+` register, otherwise to `"`.
- `.copyFilename`: Copy file name. When `clipboard='unnamed[plus]`, copies to the `+` register, otherwise to `"`.
- `.chmodx`: Run `chmod +x` on the current file.
- `.renameFile`: Rename the current file. If no extension is provided, will keep the current file extension.
- `.createNewFile`: Create a new file. If no extension is provided, will keep use the extension of the current file.
- `.duplicateFile`: Duplicate the current file. If no extension is provided, will keep the current file extension.
- `.trashFile`: Move the current file to `$HOME/.Trash`. Can optionally be passed a table to change the trash location: `.trashFile{trashLocation = "your/path/"}`. (Requires macOS or Linux, since using `mv`.) 
- `.moveSelectionToNewFile`: Visual (Line) Mode Command. Prompts for a new file name and moves the current selection to that new file. (Note that the selection is moved linewise.)

## Why that name?
A nod to [vim.eunuch](https://github.com/tpope/vim-eunuch) – as opposed to childless eunuchs, it is said that Ghengis Khan [has fathered thousands of children](https://allthatsinteresting.com/genghis-khan-children).

---

This is my very first neovim plugin, so suggestions for code improvements are welcome. 🙏
