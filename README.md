# dotfiles_bootstrap

## セットアップ手順

---

### ステップ1：SSHキー作成とGitHub登録

まずはGitHubに安全に接続するためのSSHキーを作成し、公開鍵をGitHubに登録します。

```sh
# Xcode Command Line Toolsのインストール（Homebrewやgitに必須）
xcode-select --install

# SSHキー生成とクリップボードコピー
mkdir -p ~/.ssh
ssh-keygen -t ed25519 -f ~/.ssh/github_ed25519 -N "" -C ""
cat ~/.ssh/github_ed25519.pub | pbcopy

# SSH config作成
cat <<'EOF' >> ~/.ssh/config
# GitHub用設定
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_ed25519
  IdentitiesOnly yes
EOF
chmod 600 ~/.ssh/config

# メッセージ表示と一時停止
echo "✅ 公開鍵がクリップボードにコピーされました"
echo "🚀 エンターキーを押すとGitHubのSSHキー設定ページが開きます"
echo "📝 ページで「New SSH key」をクリックして公開鍵を登録してください"
echo "⏳ 準備ができたらエンターキーを押してください"
read

# GitHubのSSHキー設定ページを自動で開く
open https://github.com/settings/keys
```


**💡 ヒント：コマンドを実行すると、自動的にGitHubのSSHキー設定ページが開きます。**

---

### ステップ2：dotfilesセットアップ

SSHキーをGitHubに登録したら、以下をまとめて実行してください。

```sh
# dotfilesリポジトリをクローン
dir=~/dev/github.com/naoHash && mkdir -p "$dir" && git clone -C "$dir" 
git@github.com:naoHash/dotfiles.git

# 実行権限を付与し実行
chmod +x ./install.sh
./install.sh
```
