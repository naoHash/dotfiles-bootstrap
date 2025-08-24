# dotfiles_bootstrap

## セットアップ手順

---

### ステップ1：SSHキー作成とGitHub登録

まずはGitHubに安全に接続するためのSSHキーを作成し、公開鍵をGitHubに登録します。

```sh
echo "🔧 Xcode Command Line Toolsをインストール中..."
xcode-select --install

echo "🔑 SSHキーを生成中..."
mkdir -p ~/.ssh
ssh-keygen -t ed25519 -f ~/.ssh/github_ed25519 -N "" -C ""
cat ~/.ssh/github_ed25519.pub | pbcopy

echo "⚙️ SSH configを作成中..."
cat <<'EOF' >> ~/.ssh/config
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_ed25519
  IdentitiesOnly yes
EOF
chmod 600 ~/.ssh/config

echo "✅ 公開鍵がクリップボードにコピーされました"
echo "🚀 エンターキーを押すとGitHubのSSHキー設定ページが開きます"
echo "📝 ページで「New SSH key」をクリックして公開鍵を登録してください"
echo "⏳ 準備ができたらエンターキーを押してください"
read

echo "🌐 GitHubのSSHキー設定ページを開いています..."
open https://github.com/settings/keys
```


**💡 ヒント：コマンドを実行すると、自動的にGitHubのSSHキー設定ページが開きます。**

---

### ステップ2：dotfilesセットアップ

SSHキーをGitHubに登録したら、以下をまとめて実行してください。

```sh
echo "📁 dotfilesリポジトリをクローン中..."
mkdir -p ~/dev/github.com/naoHash
cd ~/dev/github.com/naoHash
git clone git@github.com:naoHash/dotfiles.git

echo "🚀 dotfilesのセットアップを開始します..."
cd dotfiles
chmod +x ./install.sh
./install.sh
```
