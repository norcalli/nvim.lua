# nvim.lua

`nvim` is an object which contains shortcut/magic methods that are very useful for mappings.

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

Or if you're extreme:

```lua
if #nvim.list_bufs() > 1 then
	if not nvim.bo.modifiable then
		nvim.ex.bd()
	else
		nvim.command("w | bd")
	end
else
	nvim.ex.xit()
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

## API

### API Function and Command Shortcuts

All of these methods cache the inital lookup in the metatable, but there is a small overhead regardless.

#### nvim.$method

`nvim.$method(...)` redirects to `vim.api.nvim_$method(...)`

e.g. `nvim.command(...) == vim.api.nvim_command(...)`.

This is mostly for laziness.

#### nvim.fn.$method

`nvim.fn.$method(...)` redirects to `vim.api.nvim_call_function($method, {...})`

e.g. `nvim.fn.expand("%:h")` or `nvim.fn.has("terminal")`

#### nvim.ex.$method

`nvim.ex.$command(...)` is approximately `:$command flatten({...}).join(" ")`

e.g. `nvim.ex.edit("term://$SHELL")` or `nvim.ex.startinsert()`

NOTE: Since `!` isn't a valid identifier character, you can use `_` at the end to indicate a `!`
	e.g. `nvim.ex.nnoremap_("x", "<Cmd>echo hi<CR>")`

### Variable shortcuts

#### nvim.g 

- `nvim.g` can be used to get/set `g:` global variables.
	- e.g. `nvim.g.variable == g:variable`
	- `nvim.g.variable = 123` or `nvim.g.variable = nil` to delete the variable
	- `:h nvim_get_var` `:h nvim_set_var` `:h nvim_del_var` for more

#### nvim.v 

- `nvim.v` can be used to get/set `v:` variables.
	- e.g. `nvim.v.count1 == v:count1`
	- Useful `v:` variables, `v:register`, `v:count1`, etc..
	- `nvim.v.variable = 123` to set the value (when not read-only).
	- `:h nvim_get_vvar` `:h nvim_set_vvar` for more

#### nvim.b 

- `nvim.b` can be used to get/set `b:` buffer variables for the current buffer.
	- e.g. `nvim.b.variable == b:variable`
	- `nvim.b.variable = 123` or `nvim.b.variable = nil` to delete the variable
	- `:h nvim_buf_get_var` `:h nvim_buf_set_var` `:h nvim_buf_del_var` for more

#### nvim.env 

- `nvim.env` can be used to get/set environment variables.
	- e.g. `nvim.env.PWD == $PWD`
	- `nvim.env.TEST = 123` to set the value. Equivalent to `let $TEST = 123`.
	- `:h setreg` `:h setreg` for more. These aren't API functions.

#### nvim.o 

- `nvim.o` can be used to get/set global options, as in `:h options` which are set through `set`.
	- e.g. `nvim.o.shiftwidth == &shiftwidth`
	- `nvim.o.shiftwidth = 8` is equivalent to `set shiftwidth=8` or `let &shiftwidth = 8`
	- `:h nvim_get_option` `:h nvim_set_option` for more.

#### nvim.bo 

- `nvim.bo` can be used to get/set **buffer** options, as in `:h options` which are set through `setlocal`.
	- Only for the current buffer.
	- e.g. `nvim.bo.shiftwidth == &shiftwidth`
	- `nvim.bo.shiftwidth = 8` is equivalent to `setlocal shiftwidth=8`
	- `:h nvim_buf_get_option` `:h nvim_buf_set_option` for more.

