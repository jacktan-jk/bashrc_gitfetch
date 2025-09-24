# Bash Git Fetch Automation

This repository contains `src_bashrc`, a Bash profile script that prepares a hybrid Windows/MSYS2 development environment and orchestrates background `git fetch` status checks for every repository inside a designated projects directory.
![gitfetch-1](https://github.com/user-attachments/assets/2a2fcb4a-9632-45ae-bbdc-20f0ac7645b1)

## Features

- **Environment bootstrapping** – Normalizes `PATH` and `PKG_CONFIG_PATH` entries, exports a compact prompt history (`PROMPT_DIRTRIM`), and defines colorful ANSI helpers for later output.
- **Cross-platform Node.js helpers** – Switches between direct `node.exe` execution and `winpty`-wrapped binaries when available, while providing `npm`, `npx`, and `supabase` convenience aliases.
- **Python runtime toggle** – Detects whether to use Windows Store Python, MSYS2/MinGW Python, or the native Unix runtime and lets you flip the selection by writing to `.disable_win_py`.
- **Workspace-aware defaults** – Resolves the working tree root from `$PROJECTS_ROOT` (falling back to `~/Desktop/projects`), ensures the directory exists, and avoids re-running fetches when `.disable_fetch` is set to `1`.
- **Automated git status dashboard** – Iterates through every Git repository underneath the projects directory, runs `git fetch --all --prune`, and summarizes new commits, upstream drift, unstaged changes, stashes, and newly discovered remote branches.
- **Interactive controls** – Provides `reload` for re-sourcing the profile, `status` for inspecting the latest aggregated results, and graceful cancellation with `Ctrl+C` via signal traps.

## Getting started

1. Clone the repository somewhere persistent, for example:
   ```bash
   git clone https://example.com/bashrc_gitfetch.git
   cd bashrc_gitfetch
   ```
2. Backup and remove any existing Bash profile in your home directory so you can switch to the tracked version:
   ```bash
   [ -f "$HOME/.bashrc" ] && mv "$HOME/.bashrc" "$HOME/.bashrc.backup"
   rm -f "$HOME/.bashrc"
   ```
3. Symlink the repository-managed script into place. Future `git pull` updates will immediately apply to new shells:
   ```bash
   ln -s "$(pwd)/src_bashrc" "$HOME/.bashrc"
   ```
4. Source the profile (or open a new terminal) to load the configuration:
   ```bash
   source "$HOME/.bashrc"
   ```
5. Optional: set a custom projects directory before launching the shell session.
   ```bash
   export PROJECTS_ROOT="$HOME/code"
   ```

On start-up the script prints internet/Python diagnostics, then begins fetching repositories in the background while emitting a tree-formatted summary to `"$PROJECTS_ROOT"/.status`.

## Usage tips

- Toggle Python mode by writing `0` (Windows Store) or `1` (MSYS2/MinGW) to `.disable_win_py`, then restart your shell.
- Temporarily pause background fetches by writing `1` to `.disable_fetch`.
- Inspect previous results at any time via the `status` alias or by opening `.status` directly.
- Customize ANSI colors, aliases, and PATH additions to suit your environment.

## Requirements

- Bash 4+
- Git
- Optional: `winpty`, `attrib`, and MSYS2/MinGW toolchains when running on Windows.
