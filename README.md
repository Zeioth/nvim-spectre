# nvim-spectre
A search panel for neovim.

**Spectre** __find the enemy and replace them with dark power.__

**WIP**
![demo](./images/demo.gif)

## Why Spectre?
* Use regex in search
* It can filter search by path glob (filetype)
* It only search when you leave **Insert Mode**. You don't need a typeahead function .
It always search when you type and it is very useless on search with regex
* Use 1 buffer and you can edit or move
* A tool to replace text on project

## Installation

``` lua
Plug 'nvim-lua/popup.nvim'
Plug 'nvim-lua/plenary.nvim'
Plug 'windwp/nvim-spectre'
```
## Usage

``` vim
nnoremap <leader>S :lua require('spectre').open()<CR>

"search current word
vnoremap <leader>S :lua require('spectre').open_visual()<CR>
nnoremap <leader>Sw viw:lua require('spectre').open_visual()<CR>
"  search in current file
nnoremap <leader>sp viw:lua require('spectre').open_file_search()<cr>

```

**WARNING**
* Commit your file before you replace text. It is not support undo
* Don't use your crazy vim skill to edit result text or UI.
* You can use dd to delete result item


## Regex Issue
* default regex use vim regex **magic mode** `\v` and **no-ignore-case** .
* It has different regex of search command `rg` and replace command
`sed` so careful if you want to replace text.
* It has a different of hightlight result because I use vim regex to
hightlight text so careful but you can try to replace.

## Replace

you can replace with group by `\0-9` like vim and sed
if you run command replace and don't see the change. you need to reload
file with `:e` because `sed` is replace outside vim.

## Customize
Change any setting if you don't like it.

``` lua
require('spectre').setup({

  line_sep = '--------------------------------------',
  result_padding = '    ',
  highlight = {
    ui = "String",
    search = "DiffChange",
    replace = "DiffDelete"
  },
  mapping={
    ['delete_line'] = {
        map = "dd",
        cmd = "<cmd>lua require('spectre').delete()<CR>",
        desc = "delete current item"
    },
    ['enter_file'] = {
        map = "<cr>",
        cmd = "<cmd>lua require('spectre.actions').select_entry()<CR>",
        desc = "goto current file"
    },
    ['send_to_qf'] = {
        map = "rq",
        cmd = "<cmd>lua require('spectre.actions').send_to_qf()<CR>",
        desc = "send all item to quickfix"
    },
    ['replace_cmd'] = {
        map = "rc",
        cmd = "<cmd>lua require('spectre.actions').replace_cmd()<CR>",
        desc = "input replace vim command"
    },
    ['show_option_menu'] = {
        map = "to",
        cmd = "<cmd>lua require('spectre').show_options()<CR>",
        desc = "show option"
    },
    ['run_replace'] = {
        map = "rS",
        cmd = "<cmd>lua require('spectre.actions').run_replace()<CR>",
        desc = "replace all"
    },
    ['toggle_ignore_case'] = {
      map = "ti",
      cmd = "<cmd>lua require('spectre').change_options('ignore-case')<CR>",
      desc = "toggle ignore case"
    },
    ['toggle_ignore_hidden'] = {
      map = "th",
      cmd = "<cmd>lua require('spectre').change_options('hidden')<CR>",
      desc = "toggle search hidden"
    },
    -- you can put your mapping here it only have normal
  },
  find_engine = {
    -- rg is map wiht finder_cmd
    ['rg'] = {
      cmd = "rg",
      -- default args
      args = {
        '--color=never',
        '--no-heading',
        '--with-filename',
        '--line-number',
        '--column',
      } ,
      options = {
        ['ignore-case'] = {
          value= "--ignore-case",
          icon="[I]",
          desc="ignore case"
        },
        ['hidden'] = {
          value="--hidden",
          desc="hidden file",
          icon="[H]"
        },
        -- you can put any option you want here it can toggle with
        -- show_option function
      }
    },
  },
  replace_engine={
      ['sed']={
          cmd = "sed",
          args = nil
      },
  },
  default = {
      find = {
          --pick one of item that find_engine
          cmd = "rg",
          options = {"ignore-case"}
      },
      replace={
          --pick one of item that replace_engine
          cmd = "sed"
      }
  },
  replace_vim_cmd = "cfdo",
  is_open_target_win = true --open file on openner windown
  is_insert_mode = false,  -- start open panel on is_insert_mode
})

```
### Custom function

``` lua
-- if you want to get item from spectre panel you can use some function.
-- create your function and add it to mapping config on setup.
require('spectre.actions').get_current_entry()
require('spectre.actions').get_all_entries()
require('spectre.actions').get_state()

-- write your custom open function
require('spectre').open({
  is_insert_mode = true,
  search_text="test",
  replace_text="test",
  path="lua/**/*.lua"
})

```
## FAQ
* what is Spectre?
> I want create a `Search Panel` but this name is not cool.
> so I get the hero name from a game.
> Spectre has a skill to find enemy on global map so i use it:)
