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

詳細な使い方は[Wiki](https://github.com/nitmic/nitmic.club.nitech.ac.jp/wiki)を参照してください。

### このリポジトリをローカルに clone する

下記のコマンドを実行することでこのリポジトリをローカルに clone することができます：

```
$ git clone git@github.com:nitmic/nitmic.club.nitech.ac.jp.git
```

または GitHub Desktop や Visual Studio Code の機能を利用してください。

### 記事の追加方法
[Wiki](https://github.com/nitmic/nitmic.club.nitech.ac.jp/wiki) にある【記事の書き方】のとおりに、記事本文と画像を用意します。

基本的にはMarkdown記法による執筆が望ましいです。Dev Containerを使うとさらに便利になります。もちろん他の方法で用意してホームページ担当者にデータを投げてもよいです。

現在は、issue を立てて PR の merge によって issue を閉じる、という流れで記事を追加しています。また、メタ情報や画像の記述など、純 Markdown ではない記法に関してもWikiに記述があります。このあたりの情報が[【記事の書き方】MarkdownとGitでラクに](https://github.com/nitmic/nitmic.club.nitech.ac.jp/wiki/%E8%A8%98%E4%BA%8B%E3%81%AE%E6%9B%B8%E3%81%8D%E6%96%B9_md%E3%81%A8Git%E3%81%A7%E3%83%A9%E3%82%AF%E3%81%AB) にあるので、必ず読んでください。

画像はwebp形式を推奨しています。[Squoosh](https://squoosh.app/) 等で変換してください。

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
