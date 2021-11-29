# slow-lsp-demo

In the past time, the neovim builtin-lsp and Lua based completion plugin slows down when using the old typescript-language-server.

This problem was caused by the following reasons...


#### 1. The previous version of typescript-language-server behavior

The previous version of typescript-language-server returns toooooo many completion items.

-> Now, typescript-language-server uses `isIncomplete=true` and the payload is small as enough.


#### 2. The previous version of nvim built-in lsp was using `vim.fn.json_decode` to parse LSP payloads.

It may take a time near 100ms if the payload toooo large.

-> Now, nvim built-in lsp uses `cjson` as the JSON parser. It quit faster than old one.


#### 3. The previous version of nvim built-in lsp convert `vim.NIL` as `nil` via recursive processing.

It may take a time near 100ms if the payload toooo large.

-> Now, nvim built-in lsp converts `vim.NIL` to `nil` when parsing that is avoiding extra recursive processing.


NOTE: The completion plugin was the slowness reason too (but it isn't the core reason).


# How can I reproduce the slowness with this repo?

1. Downgrade the nvim.
2. Downgrade the typescript-language-server as `0.4.0`
3. Run `npm install`
4. Invoke completion with `<|>` on `src/index.ts` 

