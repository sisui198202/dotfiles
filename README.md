# dotfiles

macOS 環境を新しい Mac でもすぐ再現するための設定ファイル集。

## 内容

- `vscode/`
  - `settings.json`
  - `keybindings.json`
  - `tasks.json`
- `karabiner/`
  - `karabiner.json`

## 前提ツール

- Git
- VS Code
- Karabiner-Elements
- Node.js（Volta でインストールすることを想定）

例:

```bash
brew install volta
volta install node

---
## 新しい Mac でのセットアップ

```bash
cd ~/Work
git clone git@github.com:sisui198202/dotfiles.git

# VS Code ユーザー設定
cp ~/Work/dotfiles/vscode/settings.json \
   "$HOME/Library/Application Support/Code/User/settings.json"

cp ~/Work/dotfiles/vscode/keybindings.json \
   "$HOME/Library/Application Support/Code/User/keybindings.json"

cp ~/Work/dotfiles/vscode/tasks.json \
   "$HOME/Library/Application Support/Code/User/tasks.json"

# Karabiner 設定
mkdir -p "$HOME/.config/karabiner"
cp ~/Work/dotfiles/karabiner/karabiner.json \
   "$HOME/.config/karabiner/karabiner.json"
```