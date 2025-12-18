# dotfiles - macOS環境設定管理

## 概要

dotfilesディレクトリは、**macOS環境設定の一元管理とバージョン管理**を行う拠点です。VSCode、Karabiner-Elementsなどの設定ファイルをGitHub Private管理し、新しいMacでもすぐに開発環境を再現できるようにします。

### 目的
- **環境再現性**: 新PC環境を迅速にセットアップ
- **設定のバックアップ**: 重要な設定ファイルの保護
- **バージョン管理**: 設定変更履歴の追跡・ロールバック
- **共通化**: 複数Mac間での設定統一

### 管理対象
- **VSCode設定**: settings.json、keybindings.json、tasks.json
- **Karabiner-Elements設定**: karabiner.json
- **将来的拡張**: zsh設定、Git設定、その他ツール設定

### リポジトリ管理
- GitHub Private管理（SSH接続）
- リポジトリ: `git@github.com:sisui198202/dotfiles.git`

## 基本的な使い方

### ディレクトリ構造

```
/Users/user/Work/dotfiles/
├── README.md              # このファイル
├── vscode/                # VSCode設定
│   ├── settings.json      # エディタ設定
│   ├── keybindings.json   # キーバインド設定
│   ├── tasks.json         # タスク設定
│   └── extensions.txt     # 拡張機能リスト（推奨）
├── karabiner/             # Karabiner-Elements設定
│   └── karabiner.json     # キーマッピング設定
├── docs/                  # ドキュメント
│   └── setup-guide.md     # セットアップガイド（推奨）
└── .git/                  # Git管理
```

### Git Cloneとセットアップ

**初回セットアップ（新しいMac）**:
```bash
# Workディレクトリに移動
cd ~/Work

# dotfilesリポジトリをクローン
git clone git@github.com:sisui198202/dotfiles.git

# VSCode設定を適用
cp ~/Work/dotfiles/vscode/settings.json \
   "$HOME/Library/Application Support/Code/User/settings.json"

cp ~/Work/dotfiles/vscode/keybindings.json \
   "$HOME/Library/Application Support/Code/User/keybindings.json"

cp ~/Work/dotfiles/vscode/tasks.json \
   "$HOME/Library/Application Support/Code/User/tasks.json"

# Karabiner設定を適用
mkdir -p "$HOME/.config/karabiner"
cp ~/Work/dotfiles/karabiner/karabiner.json \
   "$HOME/.config/karabiner/karabiner.json"
```

### 設定ファイルの更新・保存

**設定変更後の反映**:
```bash
cd ~/Work/dotfiles/

# VSCode設定をdotfilesにコピー
cp "$HOME/Library/Application Support/Code/User/settings.json" \
   vscode/settings.json

cp "$HOME/Library/Application Support/Code/User/keybindings.json" \
   vscode/keybindings.json

cp "$HOME/Library/Application Support/Code/User/tasks.json" \
   vscode/tasks.json

# Karabiner設定をdotfilesにコピー
cp "$HOME/.config/karabiner/karabiner.json" \
   karabiner/karabiner.json

# Git管理
git add vscode/ karabiner/
git commit -m "feat: VSCode/Karabiner設定を更新"
git push origin main
```

## 実践例

### 1. VSCode設定管理

**settings.json - エディタ基本設定**:
```json
{
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "editor.formatOnSave": true,
  "files.autoSave": "afterDelay"
}
```

**keybindings.json - カスタムキーバインド**:
```json
[
  {
    "key": "cmd+shift+t",
    "command": "workbench.action.terminal.toggleTerminal"
  }
]
```

**tasks.json - カスタムタスク**:
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "npm install",
      "type": "shell",
      "command": "npm install"
    }
  ]
}
```

**extensions.txt - 拡張機能リスト（推奨追加）**:
```bash
# 拡張機能リスト生成
code --list-extensions > ~/Work/dotfiles/vscode/extensions.txt

# 新PCで一括インストール
cat ~/Work/dotfiles/vscode/extensions.txt | xargs -L 1 code --install-extension
```

### 2. Karabiner-Elements設定管理

**karabiner.json - キーマッピング設定**:
- CapsLock → Control
- 英数・かな の切り替えカスタマイズ
- アプリ固有のキーマッピング

**設定例**:
```json
{
  "global": {
    "check_for_updates_on_startup": true,
    "show_in_menu_bar": true
  },
  "profiles": [
    {
      "name": "Default profile",
      "simple_modifications": [
        {
          "from": {"key_code": "caps_lock"},
          "to": [{"key_code": "left_control"}]
        }
      ]
    }
  ]
}
```

### 3. 新PC環境構築フロー

**前提ツールインストール**:
```bash
# Homebrew（パッケージマネージャ）
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Git
brew install git

# Node.js（Volta推奨）
brew install volta
volta install node

# VSCode
brew install --cask visual-studio-code

# Karabiner-Elements
brew install --cask karabiner-elements
```

**dotfiles適用**:
上記「Git Cloneとセットアップ」の手順を実行

## ポイント（注意点・コツ）

### シンボリックリンク vs コピー

**コピー方式（現在の方式）**:
- **メリット**: シンプル、トラブルが少ない
- **デメリット**: 設定変更時に手動でdotfilesに反映必要

**シンボリックリンク方式（代替案）**:
```bash
# VSCode設定をシンボリックリンク化
ln -sf ~/Work/dotfiles/vscode/settings.json \
   "$HOME/Library/Application Support/Code/User/settings.json"
```
- **メリット**: 設定変更が自動的にdotfilesに反映
- **デメリット**: トラブル時の切り分けが複雑

**推奨**: まずコピー方式で運用、慣れたらシンボリックリンク検討

### バックアップ方針

**定期的なGit Push**:
```bash
# 設定変更後は必ずコミット・プッシュ
cd ~/Work/dotfiles/
git add -A
git commit -m "feat: 設定変更内容の簡潔な説明"
git push origin main
```

**ブランチ運用（推奨）**:
```bash
# 大きな設定変更は実験ブランチで
git checkout -b experiment/new-theme
# 設定変更・テスト
git add -A
git commit -m "experiment: 新しいテーマ設定を試行"

# 問題なければmainにマージ
git checkout main
git merge experiment/new-theme
git push origin main
```

### セキュリティ考慮事項

**機密情報の除外**:
- API Token、パスワードは dotfiles に含めない
- `.gitignore`で機密ファイルを除外

```bash
# .gitignore例
secrets/
*.key
*.pem
```

**GitHub Private管理**:
- 必ずPrivateリポジトリを使用
- SSH鍵認証を設定

### 複数Mac間の設定同期

**Mac1での設定変更**:
```bash
# 設定変更後
cd ~/Work/dotfiles/
cp "$HOME/Library/Application Support/Code/User/settings.json" vscode/
git add vscode/settings.json
git commit -m "feat: フォントサイズを変更"
git push origin main
```

**Mac2での設定同期**:
```bash
# 最新設定を取得
cd ~/Work/dotfiles/
git pull origin main

# VSCodeに反映
cp vscode/settings.json "$HOME/Library/Application Support/Code/User/settings.json"
```

## まとめ

### 押さえるべき要点
1. **GitHub Private管理**: 設定のバージョン管理・バックアップ
2. **新PC迅速セットアップ**: 環境再現性の確保
3. **定期的なコミット**: 設定変更履歴の記録
4. **セキュリティ**: 機密情報の除外

### dotfiles運用のコツ
- **小まめなコミット**: 設定変更時は即座にGit管理
- **コメント記載**: commit messageで変更理由を明記
- **テスト**: 大きな設定変更は実験ブランチで試行
- **ドキュメント**: READMEに設定の意図を記録

## 関連項目

### GitHub管理
- **リポジトリ**: git@github.com:sisui198202/dotfiles.git
- **SSH設定**: `~/.ssh/config` でGitHub接続設定

### 環境設定ツール
- **VSCode**: https://code.visualstudio.com/
- **Karabiner-Elements**: https://karabiner-elements.pqrs.org/
- **Volta**: https://volta.sh/

### Work内の関連ディレクトリ
- **Automation/**: 自動化スクリプト（環境セットアップ自動化の検討）
- **Documents/Summary/**: dotfiles運用ノウハウの記録

### dotfiles運用ベストプラクティス
- **参考**: https://dotfiles.github.io/
- **Awesome Dotfiles**: https://github.com/webpro/awesome-dotfiles

## 次のステップ

### 新規ツール設定追加

**zsh設定の追加（例）**:
```bash
# dotfiles/zsh/ディレクトリ作成
mkdir -p ~/Work/dotfiles/zsh/

# .zshrc をコピー
cp ~/.zshrc ~/Work/dotfiles/zsh/.zshrc

# Git管理
cd ~/Work/dotfiles/
git add zsh/
git commit -m "feat: zsh設定を追加"
git push origin main
```

**新PCでの適用**:
```bash
cp ~/Work/dotfiles/zsh/.zshrc ~/.zshrc
source ~/.zshrc
```

### extensions.txt 管理の追加

**VSCode拡張機能リスト化**:
```bash
# 現在の拡張機能をリスト化
code --list-extensions > ~/Work/dotfiles/vscode/extensions.txt

# Git管理
cd ~/Work/dotfiles/
git add vscode/extensions.txt
git commit -m "feat: VSCode拡張機能リストを追加"
git push origin main
```

**新PCで一括インストール**:
```bash
cat ~/Work/dotfiles/vscode/extensions.txt | xargs -L 1 code --install-extension
```

### セットアップスクリプト自動化

**setup.sh の作成（推奨）**:
```bash
#!/bin/bash
# ~/Work/dotfiles/setup.sh

# VSCode設定
cp vscode/*.json "$HOME/Library/Application Support/Code/User/"

# Karabiner設定
mkdir -p "$HOME/.config/karabiner"
cp karabiner/karabiner.json "$HOME/.config/karabiner/"

# 拡張機能インストール
cat vscode/extensions.txt | xargs -L 1 code --install-extension

echo "dotfiles setup completed!"
```

**実行**:
```bash
cd ~/Work/dotfiles/
chmod +x setup.sh
./setup.sh
```

### トラブルシューティング

**設定が反映されない場合**:
```bash
# 1. ファイルパスの確認
ls -la "$HOME/Library/Application Support/Code/User/"

# 2. VSCode再起動

# 3. 設定ファイルの内容確認
cat "$HOME/Library/Application Support/Code/User/settings.json"
```

**Git Push失敗の場合**:
```bash
# SSH鍵の確認
ssh -T git@github.com

# リモートURLの確認
git remote -v

# 必要に応じてSSH鍵再設定
ssh-keygen -t ed25519 -C "your_email@example.com"
```

---

**最終更新**: 2025-12-16
**管理者**: macOS環境設定全体の管理
**リポジトリ**: git@github.com:sisui198202/dotfiles.git (Private)
