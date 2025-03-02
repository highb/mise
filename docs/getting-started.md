# Getting Started

This will show you how to install mise and get started with it. This is a suitable way when using an interactive shell like `bash`, `zsh`, or `fish`.

## 1. Install `mise` CLI

See [installing mise](/installing-mise) for other ways to install mise (`macport`, `apt`, `yum`, `nix`, etc.).

:::tabs key:installing-mise
== Linux/macOS

```shell
curl https://mise.run | sh
```

By default, mise will be installed to `~/.local/bin` (this is simply a suggestion. `mise` can be installed anywhere).
You can verify the installation by running:

```shell
~/.local/bin/mise --version
# mise 2024.x.x
```

- `~/.local/bin` does not need to be in `PATH`. mise will automatically add its own directory to `PATH`
  when activated.

== Brew
Using [brew](https://brew.sh/) package manager

```shell
brew install mise
```

== Windows
::: code-group

```shell [winget]
winget install jdx.mise
```

```shell [scoop]
# https://github.com/ScoopInstaller/Main/pull/6374
scoop install mise
```

== Debian/Ubuntu (apt)

For installation on Ubuntu/Debian:

::: code-group

```sh [amd64]
sudo apt update -y && sudo apt install -y gpg sudo wget curl
sudo install -dm 755 /etc/apt/keyrings
wget -qO - https://mise.jdx.dev/gpg-key.pub | gpg --dearmor | sudo tee /etc/apt/keyrings/mise-archive-keyring.gpg 1> /dev/null
echo "deb [signed-by=/etc/apt/keyrings/mise-archive-keyring.gpg arch=amd64] https://mise.jdx.dev/deb stable main" | sudo tee /etc/apt/sources.list.d/mise.list
sudo apt update
sudo apt install -y mise
```

```sh [arm64]
sudo apt update -y && sudo apt install -y gpg sudo wget curl
sudo install -dm 755 /etc/apt/keyrings
wget -qO - https://mise.jdx.dev/gpg-key.pub | gpg --dearmor | sudo tee /etc/apt/keyrings/mise-archive-keyring.gpg 1> /dev/null
echo "deb [signed-by=/etc/apt/keyrings/mise-archive-keyring.gpg arch=arm64] https://mise.jdx.dev/deb stable main" | sudo tee /etc/apt/sources.list.d/mise.list
sudo apt update
sudo apt install -y mise
```

:::

`mise` respects [`MISE_DATA_DIR`](/configuration) and [`XDG_DATA_HOME`](/configuration) if you'd like
to change these locations.

## 2. Activate `mise`

Now that `mise` is installed, you can optionally activate it or add its [shims](dev-tools/shims.md) to `PATH`.

- [`mise activate`](/cli/activate) method updates your environment variable and `PATH` every time your prompt is run to ensure you use the correct versions.
- [Shims](dev-tools/shims.md) are symlinks to the `mise` binary that intercept commands and load the appropriate environment

For interactive shells, `mise activate` is recommended. In non-interactive sessions, like CI/CD, IDEs, and scripts, using `shims` might work best. You can also not use any and call `mise exec/run` directly instead.
See [this guide](dev-tools/shims.md) for more information.

:::tabs key:installing-mise

== Linux/macOS

::: code-group

```sh [bash]
echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc
```

```sh [zsh]
echo 'eval "$(~/.local/bin/mise activate zsh)"' >> ~/.zshrc
```

```sh [fish]
echo '~/.local/bin/mise activate fish | source' >> ~/.config/fish/config.fish
```

- Make sure you restart your shell session after modifying your rc file in order for it to take
  effect.
- Also note that
  this uses `~/.local/bin/mise` as the binary location since that's what <https://mise.run> uses by
  default. If you've
  installed mise by some other means it may be on `PATH` or somewhere different.

== Brew

::: code-group

```sh [bash]
echo 'eval "$(/opt/homebrew/bin/mise activate bash)"' >> ~/.bashrc
```

```sh [zsh]
echo 'eval "$(/opt/homebrew/bin/mise activate zsh)"' >> ~/.zshrc
```

- Activation will be handled automatically if you use `fish` shell and installed via `homebrew`. This can be disabled with `set -Ux MISE_FISH_AUTO_ACTIVATE 0`.
- Make sure you restart your shell session after modifying your rc file in order for it to take effect.

== Windows

Only shims are supported for now on Windows.

- When using `scoop`, the shims are automatically added to your `PATH`.
- With `winget`, you need to add `<homedir>\AppData\Local\mise\shims` to your `PATH`.

  ```powershell
  $shimPath = "$env:USERPROFILE\AppData\Local\mise\shims"
  $currentPath = [Environment]::GetEnvironmentVariable('Path', 'User')
  $newPath = $currentPath + ";" + $shimPath
  [Environment]::SetEnvironmentVariable('Path', $newPath, 'User')
  ```

== Debian/Ubuntu (apt)

::: code-group

```sh [bash]
echo 'eval "$(mise activate bash)"' >> ~/.bashrc
```

```sh [zsh]
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc
```

```sh [fish]
echo 'mise activate fish | source' >> ~/.config/fish/config.fish
```

:::

## 3. Using `mise`

:::info
Of course, if using mise solely for [environment management](/environments/)
or [running tasks](/tasks/)
this step is not necessary. You can use it to make sure `mise` is correctly setup.
:::

As an example, here is how you can install `node` and set it as the global default:

```sh
mise use --global node@22
```

You can now run `node` using `mise exec`:

```sh
mise exec -- node -v
# v22.x.x
```

:::tip
Use `mise x -- node -v` or set a shell alias in your shell's rc file like `alias x="mise x --"` to
save some keystrokes.
:::

If you did activate `mise` or add its shims to `PATH`, then `node` is also available directly!

```sh
node -v
# v22.x.x
```

Note that when you ran `mise use --global node@22`, `mise` updated the global `mise` configuration.

```toml [~/.config/mise/config.toml]
[tools]
node = "22"
```

Follow the [walkthrough](/walkthrough) for more examples on how to use mise.

### Set up the autocompletion

See [autocompletion](/installing-mise.html#autocompletion) to learn how to set up autocompletion for your shell.
