# iTerm2-config

![Woman at a Window](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Caspar_David_Friedrich_018.jpg/700px-Caspar_David_Friedrich_018.jpg)

*"Woman at a Window" (1822) by Caspar David Friedrich — [Wikipedia](https://en.wikipedia.org/wiki/Woman_at_a_Window_(Friedrich))*

**Even my terminal deserves version control therapy.**

## About

A version-controlled iTerm2 profile, born from a 30-minute debugging session to figure out why `Alt+W` (Emacs copy) was not working. The fix: a single boolean. This repo exists so that setting never gets lost again.

The core philosophy is simple -- selecting text should copy it. Combined with the Emacs keybindings in the [zshrc](https://github.com/pdelfino/zshrc) config, this creates a fully keyboard-driven terminal where kill/yank operations and mouse selection both feed into the same system clipboard.

## The One Setting That Matters

```bash
defaults write com.googlecode.iterm2 CopySelection -bool true
```

"Copy to pasteboard on selection" -- so selecting IS copying. That is the setting this repo exists to preserve.

## What's Here

- **Default.json** -- Full iTerm2 profile exported as JSON, including colors, fonts, and key mappings

## Installation

```bash
git clone git@github.com:pdelfino/iterm2-config.git ~/projects/iterm2-config
```

To import: iTerm2 > Settings > Profiles > Other Actions > Import JSON Profiles, then select `Default.json`.

## Exporting an Updated Profile

If you change your iTerm2 settings and want to update the repo:

```python
python3 -c "
import plistlib, json, base64

with open('$HOME/Library/Preferences/com.googlecode.iterm2.plist', 'rb') as f:
    plist = plistlib.load(f)

def make_serializable(obj):
    if isinstance(obj, bytes):
        return base64.b64encode(obj).decode('ascii')
    elif isinstance(obj, dict):
        return {k: make_serializable(v) for k, v in obj.items()}
    elif isinstance(obj, list):
        return [make_serializable(v) for v in obj]
    return obj

for p in plist.get('New Bookmarks', []):
    name = p.get('Name', 'profile')
    with open(f'{name}.json', 'w') as f:
        json.dump(make_serializable(p), f, indent=2)
"
```

## Related

| Repo | What it controls |
|------|------------------|
| [emacs-config](https://github.com/pdelfino/emacs-config) | Emacs setup with Ivy, Paredit, Claude Code integration |
| [karabiner-config](https://github.com/pdelfino/karabiner-config) | Emacs keybindings system-wide on macOS |
| [homerow-config](https://github.com/pdelfino/homerow-config) | Clickless navigation -- label everything, click nothing |
| [zshrc](https://github.com/pdelfino/zshrc) | ZSH config with Emacs bindings and clipboard sync |
| [claude-config](https://github.com/pdelfino/claude-config) | Claude Code configuration |
| [macos-setup](https://github.com/pdelfino/macos-setup) | The bootstrap that ties it all together |

## License

Personal configuration -- use freely.
