# Installation Guide

## Requirements

- macOS or Linux (WSL works too)
- [Claude Code](https://claude.ai/download) installed
- `rsync` installed (`brew install rsync` on macOS)

---

## Setup

Clone or download this repo, then run:

```bash
bash setup-claude-accounts.sh
```

The script will:

1. Ask how many accounts you want (2 or 3)
2. Ask for a name for each account (e.g. `personal`, `work`, `freelance`)
3. Open the Claude login flow for each account so you can authenticate
4. Save a backup per account
5. Add shell aliases to your `~/.zshrc` or `~/.bashrc`

---

## Daily usage

After setup, reload your shell and switch accounts with:

```bash
source ~/.zshrc

use-personal     # switch to personal account
use-work         # switch to work account
use-freelance    # switch to freelance account (if configured)
```

Restart VS Code after switching for the Claude Code extension to pick up the new session.

---

## How it works

A marker file at `~/.claude_current` tracks the active account.

When you run `use-work`:

```
1. Read ~/.claude_current → e.g. "personal"
2. Save ~/.claude/ → ~/.claude_personal/   (backup current account)
3. Restore ~/.claude_work/ → ~/.claude/    (activate work account)
4. Write "work" → ~/.claude_current
```

Each account's backup lives in:

```
~/.claude_<name>/       ← config directory backup
~/.claude_<name>.json   ← session file backup
```

---

## Manual setup (without the script)

If you prefer to configure everything by hand:

**1. Log in to your first account and save a backup:**

```bash
claude auth login
rsync -a ~/.claude/ ~/.claude_<name1>/
cp ~/.claude.json ~/.claude_<name1>.json
```

**2. Log in to your second account and save a backup:**

```bash
claude auth login
rsync -a ~/.claude/ ~/.claude_<name2>/
cp ~/.claude.json ~/.claude_<name2>.json
```

**3. Add the switch function and aliases to your `~/.zshrc` (or `~/.bashrc`):**

```bash
_claude_switch() {
  local target="$1"
  local marker="$HOME/.claude_current"
  local current="$(cat "$marker" 2>/dev/null)"

  if [[ "$target" == "$current" ]]; then
    echo "Account '$target' is already active."
    return 0
  fi

  if [[ -n "$current" ]]; then
    rsync -a --ignore-errors --delete "$HOME/.claude/" "$HOME/.claude_${current}/" && \
    cp "$HOME/.claude.json" "$HOME/.claude_${current}.json"
    echo "Saved account '$current'."
  fi

  rsync -a --ignore-errors --delete "$HOME/.claude_${target}/" "$HOME/.claude/" && \
  cp "$HOME/.claude_${target}.json" "$HOME/.claude.json"
  echo "$target" > "$marker"

  echo "Account '$target' is now active. Restart VS Code to apply."
}

alias use-<name1>='_claude_switch <name1>'
alias use-<name2>='_claude_switch <name2>'
```

Replace `<name1>` and `<name2>` with your chosen account names.

**4. Reload your shell:**

```bash
source ~/.zshrc
```

**5. Mark which account is currently active:**

```bash
echo "<name2>" > ~/.claude_current
```

---

## FAQ

**Do I need to run setup again after restarting my computer?**
No. The backups persist. Only run the aliases when you want to switch accounts.

**What if I forget to switch before closing the terminal?**
The alias always saves the current state before switching, so you won't lose anything.

**Does it work on Windows?**
Not natively. It works on WSL. A PowerShell port would need adapting.

**Can I add a fourth account later?**
Log in manually with `claude auth login`, save the backup:

```bash
rsync -a ~/.claude/ ~/.claude_<name>/
cp ~/.claude.json ~/.claude_<name>.json
```

Then add to your shell config:

```bash
alias use-<name>='_claude_switch <name>'
```
