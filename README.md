# dotfiles_bootstrap

## セットアップ手順

---

### macOS/Linux用

#### ステップ1：SSHキー作成とGitHub登録

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

#### ステップ2：dotfilesセットアップ

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

---

### Windows用

#### ステップ1：SSHキー作成とGitHub登録

まずはGitHubに安全に接続するためのSSHキーを作成し、公開鍵をGitHubに登録します。

```powershell
Write-Host "🔧 Windows用の開発環境を確認中..." -ForegroundColor Green

# Gitがインストールされているかチェック
if (!(Get-Command git -ErrorAction SilentlyContinue)) {
    Write-Host "📦 Gitがインストールされていません。自動インストールを試行します..." -ForegroundColor Yellow
    
    # wingetが利用可能かチェック
    if (Get-Command winget -ErrorAction SilentlyContinue) {
        Write-Host "🚀 wingetを使用してGit for Windowsをインストール中..." -ForegroundColor Green
        try {
            winget install --id Git.Git -e --source winget
            Write-Host "✅ Git for Windowsのインストールが完了しました" -ForegroundColor Green
            Write-Host "🔄 PowerShellを再起動してから、このスクリプトを再実行してください" -ForegroundColor Yellow
            exit
        }
        catch {
            Write-Host "❌ wingetでのインストールに失敗しました" -ForegroundColor Red
        }
    }
    
    # wingetが利用できない場合のフォールバック
    Write-Host "💡 手動インストールが必要です。Git for Windowsのダウンロードページを開きます..." -ForegroundColor Yellow
    Write-Host "🚀 Git for Windowsのダウンロードページを開いています..." -ForegroundColor Green
    Start-Process "https://git-scm.com/download/win"
    exit
}

Write-Host "✅ Gitがインストールされています" -ForegroundColor Green

Write-Host "🔑 SSHキーを生成中..." -ForegroundColor Green
if (!(Test-Path "$env:USERPROFILE\.ssh")) {
    New-Item -ItemType Directory -Path "$env:USERPROFILE\.ssh" -Force
}

ssh-keygen -t ed25519 -f "$env:USERPROFILE\.ssh\github_ed25519" -N '""' -C '""'
Get-Content "$env:USERPROFILE\.ssh\github_ed25519.pub" | Set-Clipboard

Write-Host "⚙️ SSH configを作成中..." -ForegroundColor Green
$sshConfig = @"
Host github.com
  HostName github.com
  User git
  IdentityFile $env:USERPROFILE\.ssh\github_ed25519
  IdentitiesOnly yes
"@
Add-Content -Path "$env:USERPROFILE\.ssh\config" -Value $sshConfig

Write-Host "✅ 公開鍵がクリップボードにコピーされました" -ForegroundColor Green
Write-Host "🚀 エンターキーを押すとGitHubのSSHキー設定ページが開きます" -ForegroundColor Green
Write-Host "📝 ページで「New SSH key」をクリックして公開鍵を登録してください" -ForegroundColor Green
Write-Host "⏳ 準備ができたらエンターキーを押してください" -ForegroundColor Yellow
Read-Host

Write-Host "🌐 GitHubのSSHキー設定ページを開いています..." -ForegroundColor Green
Start-Process "https://github.com/settings/keys"
```

#### ステップ2：dotfilesセットアップ

SSHキーをGitHubに登録したら、以下をまとめて実行してください。

```powershell
Write-Host "📁 dotfilesリポジトリをクローン中..." -ForegroundColor Green
if (!(Test-Path "$env:USERPROFILE\dev\github.com\naoHash")) {
    New-Item -ItemType Directory -Path "$env:USERPROFILE\dev\github.com\naoHash" -Force
}
Set-Location "$env:USERPROFILE\dev\github.com\naoHash"
git clone git@github.com:naoHash/dotfiles.git

Write-Host "🚀 dotfilesのセットアップを開始します..." -ForegroundColor Green
Set-Location dotfiles
.\install.ps1
```
