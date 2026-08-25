# Neovim SDK for Workshop

This SDK provides the Neovim editor and Lua runtime for building and testing
Neovim plugins. Neovim caches are persisted on the host to speed up plugin
installs across workshop updates. It can also be used as the editing
environment inside the workshop.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: neovim-plugin
base: ubuntu@24.04
sdks:
  - name: neovim
    channel: 0.12/stable

actions:
  get-version: |
    nvim --headless -u NONE -c "lua print(vim.version())" -c "qa"
```

This demonstrates a basic invocation of a Lua script within neovim.

---

## Using the SDK

### Prerequisites, project layout

1. No prerequisite SDKs are required.
2. Your Neovim plugin project should be in your project directory. A typical
   layout looks like:

```bash
git clone <YOUR_REPO_URL>
```

3. On launch, the SDK adds the Neovim binary to `PATH`. No plugin installation
   happens automatically; dependencies are fetched when you run your plugin
   manager or test command.

### Run plugin tests

Once the workshop is ready:

```bash
workshop shell
nvim --headless -u tests/minimal_init.lua \
  -c "PlenaryBustedDirectory tests/ { minimal_init = 'tests/minimal_init.lua' }" \
  -c "qa!"
```

Neovim's `~/.cache/nvim` directory is mapped from your host via the
`nvim-cache` mount plug. Plugin manager caches and other Neovim data stored
there persist across workshop updates, so subsequent test runs and plugin
installs are faster.

To see where the cache is stored on the host:

```bash
workshop info
```

__Note:__ This SDK does not install `plenary`, it must be provided through some
other mechanism.

### Edit with Neovim

From within the workshop shell:

```bash
workshop shell
nvim lua/my_plugin/init.lua
```

The editor behaves like a native Neovim installation, with access to the
plugin project and persisted caches.

---

## Plugs (resources this SDK consumes)

### `nvim-cache`

- Interface: `mount`
- Workshop target: `/home/workshop/.cache/nvim`
- Purpose: Persists Neovim caches, including plugin manager data and state,
  between workshop updates.

## Slots (resources this SDK provides)

This SDK doesn't define any slots.

---

## Documentation and guidance

- [Neovim official documentation](https://neovim.io/doc/user/)
- [Neovim Lua guide](https://neovim.io/doc/user/lua.html)
- [Workshop documentation](https://ubuntu.com/workshop/docs/)

---

## Community and support

- Neovim community: [Neovim Discussions](https://github.com/neovim/neovim/discussions)
- Workshop forum: [Discourse](https://discourse.ubuntu.com/)
- Please review our [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct)
  before participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- Open issues or pull requests on the [official repository](https://github.com/andogq/neovim-sdk).

---

## License and copyright

Copyright 2026 Tom Anderson.

This SDK is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0),
the same license as [Neovim](https://github.com/neovim/neovim).
