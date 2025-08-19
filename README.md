# NITMic 公式 Web サイト

![workflow](https://github.com/nitmic/nitmic.club.nitech.ac.jp/actions/workflows/build.yml/badge.svg)
![workflow](https://github.com/nitmic/nitmic.club.nitech.ac.jp/actions/workflows/deploy.yml/badge.svg)
![workflow](https://github.com/nitmic/nitmic.club.nitech.ac.jp/actions/workflows/disk_space_alert.yml/badge.svg)

URL：http://nitmic.club.nitech.ac.jp/

## Overview

- **名古屋工業大学コンピュータ俱楽部 NITMic** の公式 Web サイト開発リポジトリです
- 静的サイトジェネレーター [Hugo](https://github.com/gohugoio/hugo) を利用しています
  - 使用テーマ：[Mainroad](https://github.com/Vimux/Mainroad)
- ホスティングは課外活動用ウェブサイトホスティングサービスを利用しています
  - 詳細：[国立大学法人名古屋工業大学 情報基盤センター](https://www.cc.nitech.ac.jp/service/students/web-hosting-club.html)（学内のみからアクセス）

## Usage

### このリポジトリをローカルに clone する

下記のコマンドを実行することでこのリポジトリをローカルに clone することができます：

```
$ git clone git@github.com:nitmic/nitmic.club.nitech.ac.jp.git
```

### Dev Container で開く

本リポジトリでは、[Visual Studio Code Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers) を用いた開発環境の構築を推奨しています。
Dev Containers を利用することで、Hugo や Go ランタイムなどの必要なツールを自動でセットアップでき、すべての開発者が一貫した環境で作業を行うことが可能になります。

VS Code に [Dev Containers 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) をインストールした状態でリポジトリを開くと、「Reopen in Container（コンテナで再オープン）」という通知が表示されます。
このボタンをクリックすることで、Dev Containers 環境が起動し、すぐに開発を始めることができます。

通知が表示されない場合は、コマンドパレット（`F1` または `Ctrl+Shift+P`）を開き、「Dev Containers: Reopen in Container」を選択してください。

### ローカルでプレビューする

以下のコマンドを実行すると、Hugo によるローカルサーバーが立ち上がり、サイトのプレビューを確認できます：

```
$ hugo server
```

実行後、以下のようなログが表示されます：

```
vscode ➜ /workspaces/nitmic.club.nitech.ac.jp (main) $ hugo server

hugo: collected modules in 1239 ms
Watching for changes in /workspaces/nitmic.club.nitech.ac.jp/{archetypes,assets,content,data,layouts,static}
Watching for config changes in /workspaces/nitmic.club.nitech.ac.jp/config.toml
Start building sites …
hugo v0.115.4-dc9524521270f81d1c038ebbb200f0cfa3427cc5 linux/amd64 BuildDate=2023-07-20T06:49:57Z VendorInfo=gohugoio


                   | EN
-------------------+------
  Pages            | 163
  Paginator pages  |   9
  Non-page files   |   0
  Static files     | 206
  Processed images |   0
  Aliases          |  66
  Sitemaps         |   1
  Cleaned          |   0

Built in 5244 ms
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

お使いのブラウザで http://localhost:1313/ にアクセスすると、ローカル環境でサイトのプレビューが表示されます。
編集内容は保存するたびに自動で反映されるため、リアルタイムに確認しながら作業が可能です。

## Deploy

### ホスティングサービス

NITMic 公式サイトは、大学の提供する [課外活動用ウェブサイトホスティングサービス](https://www.cc.nitech.ac.jp/service/students/web-hosting-club.html) を利用してホスティングされています。
くわしくは NITMic の Cosense（旧 Scrapbox）を参照してください。

### 自動デプロイの設定

NITMic 公式サイトは `main` ブランチが更新される度に自動でデプロイされるように設定されています。
具体的には、GitHub Actions により次のような流れでデプロイが行われます：

1. `main` ブランチが更新される
2. Build ワークフローにより GitHub Hosted Runner でビルド結果を含む Release を作成する
3. Deploy ワークフローにより Self Hosted Runner で最新の Release に含まれるビルド結果をホスティングサーバーにデプロイする
4. Disk Space Alert ワークフローによりサーバーのディスク容量を確認する

### Self Hosted Runner の起動方法

> [!NOTE]
> Self Hosted Runner については [公式ドキュメント](https://docs.github.com/ja/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners) を参照してください。

NITMic 公式サイトの Deploy ワークフローは、ホスティングサーバー上の Self Hosted Runner で実行しています。
これは、ほかの方法では VPN 接続が必要になり自動化が困難だったからです。
なんらかの原因で Self Hosted Runner が停止した場合、ホスティングサービスの管理画面の「SSH ターミナル」を開きホームディレクトリで次のコマンドを実行し Self Hosted Runner を起動してください：

```bash
export LD_LIBRARY_PATH=/usr/lib64/c++-plesk-13.2.0/lib64:$LD_LIBRARY_PATH && nohup ./actions-runner/run.sh > /dev/null 2>&1 &
```

`export LD_LIBRARY_PATH=...` は libstdc++ ライブラリのパスを通すためのコマンドです。
Self Hosted Runner の実行には libstdc++ が必要ですが、ホスティングサーバーではデフォルトでパスが通っていません。
そのため、`export` コマンドを実行して一時的にパスを通すという処理を行っています。

`nohup` コマンドは、コマンドをバックグラウンドで実行するためのコマンドです。
権限の問題で Self Hosted Runner をホスティングサーバー上でサービスとして設定できないため、`nohup` コマンドを使用してバックグラウンドで実行しています。
`> /dev/null 2>&1 &` もバックグラウンドで実行するための設定で、標準出力と標準エラー出力を `/dev/null` にリダイレクトするという処理を行っています。

### ディスク容量の注意点

> [!CAUTION]
> NITMic のアカウントは 4 GB のディスク容量制限があり、もしディスク容量を超過するとホスティングサービスが一時停止されます。

Self Hosted Runner には自動アップデート機能があり、アップデート時には一時的に 500 MB ～ 1 GB 程度のディスク容量を必要とします。
ディスク容量が 3 GB を越えたら警告を出すように Disk Space Alert ワークフローを設定しています。
もしワークフローで警告が出たら、すぐに不要なファイルを削除するなどしてディスク容量を確保してください。
万が一容量超過でホスティングサービスが一時停止された場合は、情報基盤センターにメールをして復旧を依頼してください。
また、必要に応じてディスク容量の増量を依頼してください。

> [!NOTE]
> 課外活動用ウェブサイトホスティングサービスのディスク容量は通常 500 MB ですが、NITMic では幾度かの容量超過を経て 4 GB まで増量されています。
