---
id: plugin_defaults
title: Default Plugin Configurations
---

This page documents the default options that are set by AstroNvim for each individual plugin. All of these options would go in the `plugins` table in the `user/init.lua` configuration file.

## Aerial

```lua
aerial = {
  close_behavior = "global",
  backends = { "lsp", "treesitter", "markdown" },
  min_width = 28,
  show_guides = true,
  filter_kind = {
    "Array",
    "Boolean",
    "Class",
    "Constant",
    "Constructor",
    "Enum",
    "EnumMember",
    "Event",
    "Field",
    "File",
    "Function",
    "Interface",
    "Key",
    "Method",
    "Module",
    "Namespace",
    "Null",
    "Number",
    "Object",
    "Operator",
    "Package",
    "Property",
    "String",
    "Struct",
    "TypeParameter",
    "Variable",
  },
  icons = {
    Array = "",
    Boolean = "⊨",
    Class = "",
    Constant = "",
    Constructor = "",
    Key = "",
    Function = "",
    Method = "ƒ",
    Namespace = "",
    Null = "NULL",
    Number = "#",
    Object = "⦿",
    Property = "",
    TypeParameter = "𝙏",
    Variable = "",
    Enum = "ℰ",
    Package = "",
    EnumMember = "",
    File = "",
    Module = "",
    Field = "",
    Interface = "ﰮ",
    String = "𝓐",
    Struct = "𝓢",
    Event = "",
    Operator = "+",
  },
  guides = {
    mid_item = "├ ",
    last_item = "└ ",
    nested_top = "│ ",
    whitespace = "  ",
  },
  on_attach = function(bufnr)
    -- Jump forwards/backwards with '{' and '}'
    vim.keymap.set("n", "{", "<cmd>AerialPrev<cr>", { buffer = bufnr, desc = "Jump backwards in Aerial" })
    vim.keymap.set("n", "}", "<cmd>AerialNext<cr>", { buffer = bufnr, desc = "Jump forwards in Aerial" })
    -- Jump up the tree with '[[' or ']]'
    vim.keymap.set("n", "[[", "<cmd>AerialPrevUp<cr>", { buffer = bufnr, desc = "Jump up and backwards in Aerial" })
    vim.keymap.set("n", "]]", "<cmd>AerialNextUp<cr>", { buffer = bufnr, desc = "Jump up and forwards in Aerial" })
  end,
},
```

## Autopairs

```lua
autopairs = {
  check_ts = true,
  ts_config = {
    lua = { "string", "source" },
    javascript = { "string", "template_string" },
    java = false,
  },
  disable_filetype = { "TelescopePrompt", "spectre_panel" },
  fast_wrap = {
    map = "<M-e>",
    chars = { "{", "[", "(", '"', "'" },
    pattern = string.gsub([[ [%'%"%)%>%]%)%}%,] ]], "%s+", ""),
    offset = 0,
    end_key = "$",
    keys = "qwertyuiopzxcvbnmasdfghjkl",
    check_comma = true,
    highlight = "PmenuSel",
    highlight_grey = "LineNr",
  },
},
```

## Better Escape

```lua
better_escape = {},
```

## Bufferline

```lua
bufferline = {
  options = {
    offsets = {
      { filetype = "NvimTree", text = "", padding = 1 },
      { filetype = "neo-tree", text = "", padding = 1 },
      { filetype = "Outline", text = "", padding = 1 },
    },
    buffer_close_icon = "",
    modified_icon = "",
    close_icon = "",
    show_close_icon = true,
    left_trunc_marker = "",
    right_trunc_marker = "",
    max_name_length = 14,
    max_prefix_length = 13,
    tab_size = 20,
    show_tab_indicators = true,
    enforce_regular_tabs = false,
    view = "multiwindow",
    show_buffer_close_icons = true,
    separator_style = "thin",
    always_show_bufferline = true,
    diagnostics = false,
  },
},
```

## cmp

```lua
cmp = {
  preselect = cmp.PreselectMode.None,
  formatting = {
    fields = { "kind", "abbr", "menu" },
    format = function(_, vim_item)
      vim_item.kind = string.format("%s", kind_icons[vim_item.kind])
      return vim_item
    end,
  },
  snippet = {
    expand = function(args)
      luasnip.lsp_expand(args.body)
    end,
  },
  duplicates = {
    nvim_lsp = 1,
    luasnip = 1,
    cmp_tabnine = 1,
    buffer = 1,
    path = 1,
  },
  confirm_opts = {
    behavior = cmp.ConfirmBehavior.Replace,
    select = false,
  },
  documentation = {
    border = { "╭", "─", "╮", "│", "╯", "─", "╰", "│" },
  },
  experimental = {
    ghost_text = false,
  },
  completion = {
    keyword_length = 1,
  },
  mapping = {
    ["<C-k>"] = cmp.mapping.select_prev_item(),
    ["<C-j>"] = cmp.mapping.select_next_item(),
    ["<C-d>"] = cmp.mapping(cmp.mapping.scroll_docs(-1), { "i", "c" }),
    ["<C-f>"] = cmp.mapping(cmp.mapping.scroll_docs(1), { "i", "c" }),
    ["<C-Space>"] = cmp.mapping(cmp.mapping.complete(), { "i", "c" }),
    ["<C-y>"] = cmp.config.disable,
    ["<C-e>"] = cmp.mapping {
      i = cmp.mapping.abort(),
      c = cmp.mapping.close(),
    },
    ["<CR>"] = cmp.mapping.confirm { select = true },
    ["<Tab>"] = cmp.mapping(function(fallback)
      if cmp.visible() then
        cmp.select_next_item()
      elseif luasnip.expandable() then
        luasnip.expand()
      elseif luasnip.expand_or_jumpable() then
        luasnip.expand_or_jump()
      else
        fallback()
      end
    end, {
      "i",
      "s",
    }),
    ["<S-Tab>"] = cmp.mapping(function(fallback)
      if cmp.visible() then
        cmp.select_prev_item()
      elseif luasnip.jumpable(-1) then
        luasnip.jump(-1)
      else
        fallback()
      end
    end, {
      "i",
      "s",
    }),
  },
},
```

## Colorizer

```lua
colorizer = {
  { "*" },
  {
    RGB = true, -- #RGB hex codes
    RRGGBB = true, -- #RRGGBB hex codes
    names = false, -- "Name" codes like Blue
    RRGGBBAA = false, -- #RRGGBBAA hex codes
    rgb_fn = false, -- CSS rgb() and rgba() functions
    hsl_fn = false, -- CSS hsl() and hsla() functions
    css = false, -- Enable all css features: rgb_fn, hsl_fn, names, RGB, RRGGBB
    css_fn = false, -- Enable all CSS *functions*: rgb_fn, hsl_fn
    mode = "background", -- Set the display mode
  },
},
```

## Comment

```lua
comment = {
  pre_hook = function(ctx)
    local U = require "Comment.utils"

    local location = nil
    if ctx.ctype == U.ctype.block then
      location = require("ts_context_commentstring.utils").get_cursor_location()
    elseif ctx.cmotion == U.cmotion.v or ctx.cmotion == U.cmotion.V then
      location = require("ts_context_commentstring.utils").get_visual_start_location()
    end

    return require("ts_context_commentstring.internal").calculate_commentstring {
      key = ctx.ctype == U.ctype.line and "__default" or "__multiline",
      location = location,
    }
  end,
},
```

## Dressing

```lua
dressing = {
  input = {
    default_prompt = "➤ ",
    win_options = { winhighlight = "Normal:Normal,NormalNC:Normal" },
  },
  select = {
    backend = { "telescope", "builtin" },
    builtin = { win_options = { winhighlight = "Normal:Normal,NormalNC:Normal" } },
  },
}
```

## GitSigns

```lua
gitsigns = {
  signs = {
    add = { hl = "GitSignsAdd", text = "▎", numhl = "GitSignsAddNr", linehl = "GitSignsAddLn" },
    change = { hl = "GitSignsChange", text = "▎", numhl = "GitSignsChangeNr", linehl = "GitSignsChangeLn" },
    delete = { hl = "GitSignsDelete", text = "▎", numhl = "GitSignsDeleteNr", linehl = "GitSignsDeleteLn" },
    topdelete = {
      hl = "GitSignsDelete",
      text = "契",
      numhl = "GitSignsDeleteNr",
      linehl = "GitSignsDeleteLn",
    },
    changedelete = {
      hl = "GitSignsChange",
      text = "▎",
      numhl = "GitSignsChangeNr",
      linehl = "GitSignsChangeLn",
    },
  },
  signcolumn = true,
  numhl = false,
  linehl = false,
  word_diff = false,
  watch_gitdir = {
    interval = 1000,
    follow_files = true,
  },
  attach_to_untracked = true,
  current_line_blame = false,
  current_line_blame_opts = {
    virt_text = true,
    virt_text_pos = "eol",
    delay = 1000,
    ignore_whitespace = false,
  },
  current_line_blame_formatter_opts = {
    relative_time = false,
  },
  sign_priority = 6,
  update_debounce = 100,
  status_formatter = nil,
  max_file_length = 40000,
  preview_config = {
    border = "single",
    style = "minimal",
    relative = "cursor",
    row = 0,
    col = 1,
  },
  yadm = {
    enable = false,
  },
},
```

## Heirline

```lua
heirline = {
  { -- statusline
    hl = { fg = "fg", bg = "bg" },
    astronvim.status.component.mode(),
    astronvim.status.component.git_branch(),
    astronvim.status.component.file_info(
      (astronvim.is_available "bufferline.nvim" or vim.g.heirline_bufferline)
          and { filetype = {}, filename = false, file_modified = false }
        or nil
    ),
    astronvim.status.component.git_diff(),
    astronvim.status.component.diagnostics(),
    astronvim.status.component.fill(),
    astronvim.status.component.cmd_info(),
    astronvim.status.component.fill(),
    astronvim.status.component.lsp(),
    astronvim.status.component.treesitter(),
    astronvim.status.component.nav(),
    astronvim.status.component.mode { surround = { separator = "right" } },
  },
  { -- winbar
    static = {
      disabled = {
        buftype = { "terminal", "prompt", "nofile", "help", "quickfix" },
        filetype = { "NvimTree", "neo%-tree", "dashboard", "Outline", "aerial" },
      },
    },
    init = function(self) self.bufnr = vim.api.nvim_get_current_buf() end,
    fallthrough = false,
    { -- disable winbar for some buffer and file types
      condition = function(self)
        return vim.opt.diff:get() or astronvim.status.condition.buffer_matches(self.disabled or {})
      end,
      init = function() vim.opt_local.winbar = nil end,
    },
    -- inactive buffer winbar
    astronvim.status.component.file_info {
      condition = function() return not astronvim.status.condition.is_active() end,
      unique_path = {},
      file_icon = { hl = astronvim.status.hl.file_icon "winbar" },
      file_modified = false,
      file_read_only = false,
      hl = astronvim.status.hl.get_attributes("winbarnc", true),
      surround = false,
      update = "BufEnter",
    },
    -- active buffer winbar
    astronvim.status.component.breadcrumbs { hl = astronvim.status.hl.get_attributes("winbar", true) },
  },
  { -- bufferline
    { -- file tree padding
      condition = function(self)
        self.winid = vim.api.nvim_tabpage_list_wins(0)[1]
        return astronvim.status.condition.buffer_matches(
          { filetype = { "neo%-tree", "NvimTree" } },
          vim.api.nvim_win_get_buf(self.winid)
        )
      end,
      provider = function(self) return string.rep(" ", vim.api.nvim_win_get_width(self.winid)) end,
      hl = { bg = "tabline_bg" },
    },
    -- component for each buffer tab
    astronvim.status.heirline.make_buflist(astronvim.status.component.tabline_file_info()),
    -- fill the rest of the tabline with background color
    astronvim.status.component.fill { hl = { bg = "tabline_bg" } },
    -- tab list
    {
      -- only show tabs if there are more than one
      condition = function() return #vim.api.nvim_list_tabpages() >= 2 end,
      -- create components for each tab page
      astronvim.status.heirline.make_tablist { -- component for each tab
        provider = astronvim.status.provider.tabnr(),
        hl = function(self)
          return astronvim.status.hl.get_attributes(astronvim.status.heirline.tab_type(self, "tab"), true)
        end,
      },
      -- close button for current tab
      {
        provider = astronvim.status.provider.close_button { kind = "TabClose", padding = { left = 1, right = 1 } },
        hl = astronvim.status.hl.get_attributes("tab_close", true),
        on_click = { callback = astronvim.close_tab, name = "heirline_tabline_close_tab_callback" },
      },
    },
  },
},
```

## Indent Blankline

```lua
indent_blankline = {
  show_current_context = true,
  show_current_context_start = false,
},
```

## Indent-o-Matic

```lua
["indent-o-matic"] = {
  max_lines = 2048,
  standard_widths = { 2, 4, 8 },
},
```

## Neo-Tree

```lua
["neo-tree"] = {
  close_if_last_window = true,
  popup_border_style = "rounded",
  enable_git_status = true,
  enable_diagnostics = false,
  default_component_configs = {
    indent = {
      indent_size = 2,
      padding = 0,
      with_markers = true,
      indent_marker = "│",
      last_indent_marker = "└",
      highlight = "NeoTreeIndentMarker",
      with_expanders = false,
      expander_collapsed = "",
      expander_expanded = "",
      expander_highlight = "NeoTreeExpander",
    },
    icon = {
      folder_closed = "",
      folder_open = "",
      folder_empty = "",
      default = "",
    },
    name = {
      trailing_slash = false,
      use_git_status_colors = true,
    },
    git_status = {
      symbols = {
        added = "",
        deleted = "",
        modified = "",
        renamed = "➜",
        untracked = "★",
        ignored = "◌",
        unstaged = "✗",
        staged = "✓",
        conflict = "",
      },
    },
  },
  window = {
    position = "left",
    width = 25,
    mappings = {
      ["<2-LeftMouse>"] = "open",
      ["<cr>"] = "open",
      ["o"] = "open",
      ["S"] = "open_split",
      ["s"] = "open_vsplit",
      ["C"] = "close_node",
      ["<bs>"] = "navigate_up",
      ["."] = "set_root",
      ["H"] = "toggle_hidden",
      ["R"] = "refresh",
      ["/"] = "fuzzy_finder",
      ["f"] = "filter_on_submit",
      ["<c-x>"] = "clear_filter",
      ["a"] = "add",
      ["d"] = "delete",
      ["r"] = "rename",
      ["y"] = "copy_to_clipboard",
      ["x"] = "cut_to_clipboard",
      ["p"] = "paste_from_clipboard",
      ["c"] = "copy",
      ["m"] = "move",
      ["q"] = "close_window",
    },
  },
  nesting_rules = {},
  filesystem = {
    filtered_items = {
      visible = false,
      hide_dotfiles = true,
      hide_gitignored = false,
      hide_by_name = {
        ".DS_Store",
        "thumbs.db",
        "node_modules",
        "__pycache__",
      },
    },
    follow_current_file = true,
    hijack_netrw_behavior = "open_current",
    use_libuv_file_watcher = true,
  },
  buffers = {
    show_unloaded = true,
    window = {
      mappings = {
        ["bd"] = "buffer_delete",
      },
    },
  },
  git_status = {
    window = {
      position = "float",
      mappings = {
        ["A"] = "git_add_all",
        ["gu"] = "git_unstage_file",
        ["ga"] = "git_add_file",
        ["gr"] = "git_revert_file",
        ["gc"] = "git_commit",
        ["gp"] = "git_push",
        ["gg"] = "git_commit_and_push",
      },
    },
  },
  event_handlers = {
    {
      event = "vim_buffer_enter",
      handler = function(_)
        if vim.bo.filetype == "neo-tree" then
          vim.wo.signcolumn = "auto"
        end
      end,
    },
  },
},
```

## Packer

```lua
packer = {
  compile_path = vim.fn.stdpath "config" .. "/lua/packer_compiled.lua",
  display = {
    open_fn = function()
      return require("packer.util").float { border = "rounded" }
    end,
  },
  profile = {
    enable = true,
    threshold = 0.0001,
  },
  git = {
    clone_timeout = 300,
    subcommands = {
      update = "pull --ff-only --progress --rebase=true",
    },
  },
  auto_clean = true,
  compile_on_sync = true,
},
```

## Telescope

```lua
telescope = {
  defaults = {

    prompt_prefix = " ",
    selection_caret = "❯ ",
    path_display = { "truncate" },
    selection_strategy = "reset",
    sorting_strategy = "ascending",
    layout_strategy = "horizontal",
    layout_config = {
      horizontal = {
        prompt_position = "top",
        preview_width = 0.55,
        results_width = 0.8,
      },
      vertical = {
        mirror = false,
      },
      width = 0.87,
      height = 0.80,
      preview_cutoff = 120,
    },

    mappings = {
      i = {
        ["<C-n>"] = actions.cycle_history_next,
        ["<C-p>"] = actions.cycle_history_prev,

        ["<C-j>"] = actions.move_selection_next,
        ["<C-k>"] = actions.move_selection_previous,

        ["<C-c>"] = actions.close,

        ["<Down>"] = actions.move_selection_next,
        ["<Up>"] = actions.move_selection_previous,

        ["<CR>"] = actions.select_default,
        ["<C-x>"] = actions.select_horizontal,
        ["<C-v>"] = actions.select_vertical,
        ["<C-t>"] = actions.select_tab,

        ["<C-u>"] = actions.preview_scrolling_up,
        ["<C-d>"] = actions.preview_scrolling_down,

        ["<PageUp>"] = actions.results_scrolling_up,
        ["<PageDown>"] = actions.results_scrolling_down,

        ["<Tab>"] = actions.toggle_selection + actions.move_selection_worse,
        ["<S-Tab>"] = actions.toggle_selection + actions.move_selection_better,
        ["<C-q>"] = actions.send_to_qflist + actions.open_qflist,
        ["<M-q>"] = actions.send_selected_to_qflist + actions.open_qflist,
        ["<C-l>"] = actions.complete_tag,
      },

      n = {
        ["<esc>"] = actions.close,
        ["<CR>"] = actions.select_default,
        ["<C-x>"] = actions.select_horizontal,
        ["<C-v>"] = actions.select_vertical,
        ["<C-t>"] = actions.select_tab,

        ["<Tab>"] = actions.toggle_selection + actions.move_selection_worse,
        ["<S-Tab>"] = actions.toggle_selection + actions.move_selection_better,
        ["<C-q>"] = actions.send_to_qflist + actions.open_qflist,
        ["<M-q>"] = actions.send_selected_to_qflist + actions.open_qflist,

        ["j"] = actions.move_selection_next,
        ["k"] = actions.move_selection_previous,
        ["H"] = actions.move_to_top,
        ["M"] = actions.move_to_middle,
        ["L"] = actions.move_to_bottom,

        ["<Down>"] = actions.move_selection_next,
        ["<Up>"] = actions.move_selection_previous,
        ["gg"] = actions.move_to_top,
        ["G"] = actions.move_to_bottom,

        ["<C-u>"] = actions.preview_scrolling_up,
        ["<C-d>"] = actions.preview_scrolling_down,

        ["<PageUp>"] = actions.results_scrolling_up,
        ["<PageDown>"] = actions.results_scrolling_down,
      },
    },
  },
  pickers = {},
  extensions = {},
},
```

## ToggleTerm

```lua
toggleterm = {
  size = 10,
  open_mapping = [[<c-\>]],
  hide_numbers = true,
  shade_filetypes = {},
  shade_terminals = true,
  shading_factor = 2,
  start_in_insert = true,
  insert_mappings = true,
  persist_size = true,
  direction = "float",
  close_on_exit = true,
  shell = vim.o.shell,
  float_opts = {
    border = "curved",
    winblend = 0,
    highlights = {
      border = "Normal",
      background = "Normal",
    },
  },
},
```

## Treesitter

```lua
treesitter = {
  ensure_installed = {},
  sync_install = false,
  ignore_install = {},
  highlight = {
    enable = true,
    additional_vim_regex_highlighting = false,
  },
  context_commentstring = {
    enable = true,
    enable_autocmd = false,
  },
  autopairs = {
    enable = true,
  },
  incremental_selection = {
    enable = true,
  },
  indent = {
    enable = false,
  },
  rainbow = {
    enable = true,
    disable = { "html" },
    extended_mode = false,
    max_file_lines = nil,
  },
  autotag = {
    enable = true,
  },
},
```

## Web Devicons

```lua
["nvim-web-devicons"] = {
  c = {
    icon = "",
    color = colors.c,
    name = "c",
  },
  css = {
    icon = "",
    color = colors.css,
    name = "css",
  },
  deb = {
    icon = "",
    color = colors.deb,
    name = "deb",
  },
  Dockerfile = {
    icon = "",
    color = colors.docker,
    name = "Dockerfile",
  },
  html = {
    icon = "",
    color = colors.html,
    name = "html",
  },
  js = {
    icon = "",
    color = colors.js,
    name = "js",
  },
  kt = {
    icon = "󱈙",
    color = colors.kt,
    name = "kt",
  },
  lock = {
    icon = "",
    color = colors.lock,
    name = "lock",
  },
  lua = {
    icon = "",
    color = colors.lua,
    name = "lua",
  },
  mp3 = {
    icon = "",
    color = colors.mp3,
    name = "mp3",
  },
  mp4 = {
    icon = "",
    color = colors.mp4,
    name = "mp4",
  },
  out = {
    icon = "",
    color = colors.out,
    name = "out",
  },
  py = {
    icon = "",
    color = colors.py,
    name = "py",
  },
  ["robots.txt"] = {
    icon = "ﮧ",
    color = colors.robot,
    name = "robots",
  },
  toml = {
    icon = "",
    color = colors.toml,
    name = "toml",
  },
  ts = {
    icon = "",
    color = colors.ts,
    name = "ts",
  },
  ttf = {
    icon = "",
    color = colors.ttf,
    name = "TrueTypeFont",
  },
  rb = {
    icon = "",
    color = colors.rb,
    name = "rb",
  },
  rpm = {
    icon = "",
    color = colors.rpm,
    name = "rpm",
  },
  vue = {
    icon = "﵂",
    color = colors.vue,
    name = "vue",
  },
  woff = {
    icon = "",
    color = colors.woff,
    name = "WebOpenFontFormat",
  },
  woff2 = {
    icon = "",
    color = colors.woff2,
    name = "WebOpenFontFormat2",
  },
  xz = {
    icon = "",
    color = colors.zip,
    name = "xz",
  },
  zip = {
    icon = "",
    color = colors.zip,
    name = "zip",
  },
  jsx = {
    icon = "ﰆ",
    color = colors.jsx,
    name = "jsx",
  },
  rust = {
    icon = "",
    color = colors.rs,
    name = "rs",
  },
  jpg = {
    icon = "",
    color = colors.jpg,
    name = "jpg",
  },
  png = {
    icon = "",
    color = colors.png,
    name = "png",
  },
  jpeg = {
    icon = "",
    color = colors.jpeg,
    name = "jpeg",
  },
},
```

## Which-Key

```lua
["which-key"] = {
  plugins = {
    marks = true,
    registers = true,
    spelling = {
      enabled = true,
      suggestions = 20,
    },
    presets = {
      operators = false,
      motions = true,
      text_objects = true,
      windows = true,
      nav = true,
      z = true,
      g = true,
    },
  },
  key_labels = {},
  icons = {
    breadcrumb = "»",
    separator = "➜",
    group = "+",
  },
  popup_mappings = {
    scroll_down = "<c-d>",
    scroll_up = "<c-u>",
  },
  window = {
    border = "rounded",
    position = "bottom",
    margin = { 1, 0, 1, 0 },
    padding = { 2, 2, 2, 2 },
    winblend = 0,
  },
  layout = {
    height = { min = 4, max = 25 },
    width = { min = 20, max = 50 },
    spacing = 3,
    align = "left",
  },
  ignore_missing = true,
  hidden = { "<silent>", "<cmd>", "<Cmd>", "<CR>", "call", "lua", "^:", "^ " },
  show_help = true,
  triggers = "auto",
  triggers_blacklist = {
    i = { "j", "k" },
    v = { "j", "k" },
  },
},
```

## Window Picker

```lua
{
  other_win_hl_color = colors.grey_4
}
```