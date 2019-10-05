# nvim.lua

## Why use this module?

Here's an excerpt of functionality from my `init.lua`. Without `nvim.lua`, it looks like this:

```lua
-- Save and close the current buffer instead of all of VIM.
-- But if it's the last buffer, then save and close vim.
if #vim.api.nvim_list_bufs() > 1 then
	if not vim.api.nvim_buf_get_option(vim.api.nvim_get_current_buf(), "modifiable") then
		vim.api.nvim_command("bd")
	else
		vim.api.nvim_command("w | bd")
	end
else
	vim.api.nvim_command("xit")
end
```

Here's what it looks like with `nvim.lua`:

```lua
if #nvim.list_bufs() > 1 then
	if not nvim.bo.modifiable then
		nvim.command("bd")
	else
		nvim.command("w | bd")
	end
else
	nvim.command("xit")
end
```

## Installation

```vim
Plug 'norcalli/nvim.lua'
```

## Usage

```lua
local nvim = require 'nvim'
```

