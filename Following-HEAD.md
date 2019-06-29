The following changes may require users to update configuration, plugins, or expectations. Only **breaking changes** are mentioned here, this is not a reference for new features (see [`:help nvim`](http://neovim.io/doc/user/nvim.html) instead). 

- If you don't have Neovim, see [Building Neovim](Building-Neovim) or [Installing Neovim](Installing-Neovim).
- Use `:checkhealth` to detect and fix common problems.

------------

### 2019/06/10

[#10123](https://github.com/neovim/neovim/pull/10123) Nvim core requires [libluv](https://github.com/luvit/luv/). This may require building with `-DUSE_BUNDLED_LUV=ON` if you were previously using `-DUSE_BUNDLED_LUV=OFF`. Previously `luv` was only needed for running tests; now `nvim` statically links `libluv` in order to provide `:help lua-loop`.


### 2019/03/31

[#9819](https://github.com/neovim/neovim/pull/9819) `MenuPopupChanged` renamed to `CompleteChanged`.

### 2019/01/20

[#9526](https://github.com/neovim/neovim/pull/9526): Neovim always uses the system allocator, instead of jemalloc. 

### 2018/11/18

The Python package `neovim` was renamed to [`pynvim`](https://github.com/neovim/pynvim).

"Neovim" can mean a lot of things. It can relate to the editor, or the project, or (prior to this change) the Python module, or the Ruby gem, etc. Especially confusing was the fact that the Python side would refer to it as `neovim` whereas the Neovim side referred to it as `python-client`. Now, both sides call it `pynvim` for the greater good.

For the time being, `neovim` acts as a transitional package for the new `pynvim`. Unfortunately, due to limitations in pip, the `neovim` package cannot be safely upgraded with `pip install --upgrade neovim` on all systems.

#### Steps to update "pynvim" (formerly "neovim") Python package:

    pip uninstall neovim pynvim
    pip install pynvim

- After those steps, it is safe to `pip install neovim` again, if any third-party package has it as a formal dependency. Otherwise it shouldn't be necessary.
- `import neovim` is still supported, both in plugins and third-party software using it as a library. If `import neovim` fails, then the upgrade wasn't successful. Follow the instructions above. 

### 2018/09/22

[#9024](https://github.com/neovim/neovim/pull/9024): The meaning of the `--embed` and `--headless` flags changed to facilitate better startup behaviour with external UIs. UI embedders (any user of `nvim_ui_attach`) must start Nvim **without** `--headless`:

    nvim --embed

Embedders _not_ using the UI protocol must use:

    nvim --embed --headless

In the "`--embed` only" case Nvim waits for `nvim_ui_attach` **before continuing startup** (user config, reading buffers) so that UIs can deterministically handle (display) early messages, dialogs, etc.  The client can do other requests before `nvim_ui_attach` (e.g. `nvim_get_api_info` for feature-detection). During this pre-startup phase the user config is of course not available (similar to `--cmd`).

For most UI embedders this improves startup behavior automatically, by supporting startup messages
and swapfile dialogs. For others it is potentially a breaking change, which can be easily avoided by specifying `--headless` (which is _backwards-compatible_ for that use-case).

UIs that need additional initialization after `init.vim` can use this pattern:

- Before `nvim_ui_attach` send a single `nvim_command` request with the command `"autocmd VimEnter * call rpcrequest(1, 'vimenter')"`.
- In the `vimenter` method handler the UI can then safely execute any requests.
- Nvim will only enter normal mode when the handler returns (because `rpcrequest()` is a blocking call).

### 2018/06/10

Changes from [#7679](https://github.com/neovim/neovim/pull/7679):

- Nvim treats a stdin stream as _text_ by default:
  ```
  echo foo | nvim
  ```
  - To get the old behavior, pass stdin to `-s`:
    ```
    echo foo | nvim -s -
    ```
- **Windows only:** `nvim *.txt` (glob/wildcard expression) is [treated literally](https://github.com/neovim/neovim/pull/7679/commits/1f300e08b8c0c35b2f3d79506ae9817cd8591624). Use the `:next` (`:n`) command instead:
  ```
  nvim +"n *.txt"
  ```

### 2018/02/23
Default fillchars for 'vert' and 'sep' changed from respectively `|` and `-` to `│` and `·` when the `ambiwidth` setting is different from `double`, else old defaults apply. [#8035](https://github.com/neovim/neovim/pull/8035)

### 2017/11/18

VimL (Vimscript) compatibility: `count` is no longer an alias to `v:count`.
This helps avoid mistakes in scripts. [#7407](https://github.com/neovim/neovim/pull/7407)

### 2017/08/21

`:terminal` now starts in normal mode ([#6808](https://github.com/neovim/neovim/pull/6808)), like any other buffer, instead of terminal mode.  This avoids the cursor getting "trapped" when a terminal is started.

### 2017/05/15

The `highlight` option is now read-only ([#6737](https://github.com/neovim/neovim/pull/6737)), so that the built-in highlight groups (and their behaviors) are predictable and can be used as keys for features like `winhighlight`. 

### 2017/04/03

Default 'mouse' setting changed from `mouse=a` to `mouse=` (empty). This will change again in the future after `mouse=a` is improved. To continue using mouse, add this to your init.vim:

    set mouse=a

### 2017/04/02

Support for `$NVIM_TUI_ENABLE_CURSOR_SHAPE` was removed. Use the `guicursor` option to control cursor styling. See [FAQ](https://github.com/neovim/neovim/wiki/FAQ#how-can-i-change-the-cursor-shape-in-the-terminal). 

- To **disable** cursor style changes, set `guicursor` to empty:
  ```
  :set guicursor=
  ```
- **Note:** `guicursor` is enabled by _default_ only if Nvim is certain it won't cause problems on your terminal. If you know that cursor shaping works on your terminal, set `guicursor` in your init.vim:
  ```
  :set guicursor=n-v-c:block-Cursor/lCursor-blinkon0,i-ci:ver25-Cursor/lCursor,r-cr:hor20-Cursor/lCursor
  ```

### 2016/12/12

[#5529][5529] merged Vim's support for "partial functions" which made Nvim more strict about when the implicit `self` variable is available.  Functions that reference `self` (which is common with job callbacks) must either be declared as `dict` functions:

```vim
function! s:on_stdout(id, data, event) dict abort
```

or as part of the job options dictionary:

```vim
let s:opts = { ... }
function! s:opts.on_stdout(id, data, event) abort
```

Job callbacks also must have at least 3 parameters now. See https://github.com/neovim/neovim/issues/5763#issuecomment-266722407


### 2016/11/05

[`'encoding'`][encoding] cannot be changed to a value other than "utf-8", even during initialization. [#2905](https://github.com/neovim/neovim/pull/2905)

(Background: [#2929][2929] restricted `'encoding'` to be modifiable only during initialization. One year later, we've found no problems with UTF-8 as the internal encoding, and are now making this mandatory. This only affects the internals of Nvim, it doesn't affect file encodings or plugins.)


### 2016/02/14

`:filetype plugin indent on` and `:syntax on` are now executed by default [after your vimrc](https://github.com/neovim/neovim/blob/4bfac00aa389487c4f11d34e7a3e96e4a1116800/runtime/doc/starting.txt#L431-L444). 

* If your vimrc calls `:filetype`, Neovim will _not_ change your preference. 
* If your vimrc calls `:filetype off` or `:syntax off`, that will be respected.
* If `filetype plugin indent on` and/or `syntax on` is in your vimrc, you can delete those lines.

See `:help startup` for complete details.

### 2015/10/17

The [`'viminfo'`][viminfo] option is now an alias for [`'shada'`][shada]. [`'viminfo'`][viminfo] can no longer include `n` and it can no longer be shared with Vim.

See [#3469][3469] for more information.

[3469]: https://github.com/neovim/neovim/issues/3469
[viminfo]: http://neovim.io/doc/user/options.html#%27viminfo%27
[shada]: http://neovim.io/doc/user/options.html#%27shada%27

### 2015/07/26

The behavior of the [`mkdir()`][mkdir] function has changed:

1. Assuming /tmp/foo does not exist and /tmp can be written to,
   `mkdir('/tmp/foo/bar', 'p', 0700)` will create both /tmp/foo and /tmp/foo/bar
   with the octal permissions 0700.
   Vim's mkdir() will create /tmp/foo with 0755.
2. If you try to create an existing directory with `'p'`, such as with `mkdir('/', 'p'))`, mkdir() will silently fail. In Vim this was an error.
3. Upon failure, mkdir() related error messages now include strerror() text.

See [#3041][3041] for more information.

[mkdir]: http://neovim.io/doc/user/eval.html#mkdir%28%29
[3041]: https://github.com/neovim/neovim/pull/3041


[5529]: https://github.com/neovim/neovim/pull/5529
