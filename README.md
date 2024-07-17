# settings

linux の開発環境設定のメモ用  
WSL での設定のメモだが Mac 環境とも共有できるようにし、  
開発環境の共通化を目指したもの。

## 各ツール

### Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 最初のコマンド実行後下記コマンドの案内がでるのでそれぞれ実行する
(echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/**username**/.profile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
sudo apt-get install -y build-essential
brew install gcc
```

### Docker

```bash
# dockerが必要としているパッケージのインストール
sudo apt-get update -y && sudo apt-get install ca-certificates curl gnupg -y

# インストールに必要なGPS情報のセットアップ
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# docker関係のリポジトリを追加
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# dockerのインストール
sudo apt-get update -y && sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# ログインしているユーザーをdockerグループへ所属させ、管理者権限なしで実行できるようにする
sudo usermod -aG docker $(whoami)

# Hello World
docker run hello-world
```

### NeoVim(AstroNvim)

linux で nvim 最新 DL 方法参考  
わかっている要件: AstroNvim には NeoVim(=>8.0)  
Ubuntu は場合によっては vim や nvim のバージョンを上げる必要がある
https://qiita.com/ksh-fthr/items/48dcc42c7a805320b49a  
AstroNvim  
https://docs.astronvim.com/
インストール後は AstroNvim のホーム画面(`nvim`実行)で`:LspInstall <Language>`や`:TSInstall <Language>`で Language Server や Language Parser の設定を行う。

### cmder

settings -> Startup -> Tasks

- Add/refresh default tasks... で Tasks のリロード
- {WSL::Ubuntu}を選択し、add: Tab... Startup dir...ボタンの上のテキストエリアに`%windir%\system32\wsl.exe ~ --distribution Ubuntu`と入力する
  General
- Choose your startup tasl でデフォルトのタスクを選択する

# Nerd Fonts
FiraCode Nerd Font

## 参考にしたもの

[Windows ユーザーにささぐ、WSL2 を利用した（ちょっと便利な）Linux 開発環境作成](https://sqripts.com/2023/07/18/57951/)
