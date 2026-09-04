# sudo apt opsec

REAL sudo apt opsec linux (holy opsec reference) command to install REAL 'opsec' (those who larp).

Script works on any linux distro, with both bash and zsh

## Install

```bash
git clone https://github.com/lemonke68/opsec.git
cd opsec
./opsec install
```

`install` does two things:

1. Copies the script to `/usr/local/bin/opsec` (asks for `sudo`).
2. Appends a small `sudo` shell function to your shell's rc file — `~/.zshrc`
   if your login shell is zsh, otherwise `~/.bashrc` — so that `sudo apt opsec`
   runs the gag while every other `sudo` command passes straight through to the
   real `sudo`.

The installer prints the exact `source` command for your rc file — run it (or
just open a new shell) to load the hook before trying `sudo apt opsec`.

## Usage

```bash
sudo apt opsec     # the full experience
opsec              # same gag, without the apt wrapper
opsec uninstall    # remove the binary
```

## How the `sudo apt opsec` trick works

`apt` has no plugin system (unlike `git`, it won't dispatch to an external `apt-opsec`), and `sudo` runs a real binary, not a shell function — so you can't hook it by patching `apt` itself without touching a system-critical tool. Instead the installer defines a `sudo` **shell function** in your `~/.bashrc`:

```bash
sudo() {
  if [ "$1" = apt ] && [ "$2" = opsec ]; then command opsec; return; fi
  command sudo "$@"
}
```

It matches exactly `sudo apt opsec` and forwards everything else to the real
`sudo` via `command sudo`. It only lives in your interactive shell and never
modifies the actual `apt` or `sudo` binaries.

## Uninstall

```bash
opsec uninstall
```

Then delete the `# opsec meme hook` block from your `~/.bashrc` by hand.
