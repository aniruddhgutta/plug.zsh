# plug.zsh

**plug.zsh** is an extremely simple zsh plugin manager (~60 LOC), pre-compiling plugins to avoid startup overhead.
It can install, update and remove plugins.

---

## Setup

1. Clone `plug.zsh` into `$ZDOTDIR` (if setup in `~/.zshenv`) or any other preferred directory (eg: `$HOME/.local/share/zsh`).
In this case, we shall proceed by using `$ZDOTDIR`. Ensure that the directory exists before cloning.

```sh
git clone https://codeberg.org/oceanicc/plug.zsh "$ZDOTDIR/plug"  # or any other preferred directory
```

2. To setup `plug.zsh`, add this snippet to `.zshrc`.
You may set `$ZDATADIR` to any directory you prefer *before* calling `plug-setup`.
If unset, it defaults to `$XDG_DATA_HOME/zsh` or `$HOME/.local/share/zsh`.
```sh
plug-setup() {
    : "${ZDATADIR:=${XDG_DATA_HOME:-$HOME/.local/share}/zsh}"
    src="$ZDOTDIR/plug/plug.zsh"  # or any other preferred directory
    dir="$ZDATADIR/plug" dst="$dir/plug.zsh"
    mkdir -p "$dir"
    [ ! -e "$dir/plug.zwc" ] || [ "$src" -nt "$dst" ] && {
        cp "$src" "$dst"
        zcompile "$dir/plug.zwc" "$dst"
        print -P "%F{green}✔ Compiled plug.zsh%f"
    }
    source "$dst"
}
# ZDATADIR="$HOME/.cache/zdata"  # optionally override $ZDATADIR
plug-setup
```

---

## Installing plugins

Plugins can be installed via `plug-install`. However, plugins must be sourced manually.
Example:
```sh
# install and compile plugins
plug-install https://github.com/zsh-users/zsh-syntax-highlighting
plug-install https://github.com/zsh-users/zsh-autosuggestions
plug-install https://github.com/romkatv/powerlevel10k

# source plugins
source "$ZDATADIR/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh"
source "$ZDATADIR/zsh-autosuggestions/zsh-autosuggestions.zsh"
source "$ZDATADIR/powerlevel10k/powerlevel10k.zsh-theme"
[ ! -f "$ZDOTDIR/.p10k.zsh" ] || source "$ZDOTDIR/.p10k.zsh"
```

---

## Removing plugins

Plugins can be removed via `plug-remove`.
Make sure to remove any `plug-install` snippets for removed plugins from `~/.zshrc` to prevent `plug.zsh` from re-installing them.
Example:
```sh
plug-remove zsh-syntax-highlighting
plug-remove zsh-autosuggestions
plug-remove powerlevel10k
```

---

## Updating plugins

Plugins can be updated via `plug-update`.
Example:
```sh
plug-update zsh-syntax-highlighting
plug-update zsh-autosuggestions
plug-update powerlevel10k
```

---

## List installed plugins

Installed plugins can be viewed via `plug-list`.
```sh
plug-list
```

---

## License

[MIT](./LICENSE)
