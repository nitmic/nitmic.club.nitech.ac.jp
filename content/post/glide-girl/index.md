---
title: "GlideGirl"
date: "2022-12-01"
thumbnail: "title-anime.gif"
categories: ["作品紹介"]
description: "2022年日本ゲーム大賞「感触」向けゲーム GlideGirl"
tags: ["NITMic", "2022", "ゲーム大賞"]
authors:
  [
    "dokudami",
    "kuru",
    "hydriod",
    "Kihiro",
    "ハヤシ",
    "たくみ",
    "Taku-san",
    "ショコラ",
    "fjktkm",
    "kk",
    "塩味",
    "fibrin",
    "shenzel",
    "Mass",
    "Thike",
  ]
---

{{< load-photoswipe>}}

2020 年度入学の dokudami です。
2023 年度の部長を務めておりました。
本記事では 2022 年に NITMic 部員の 1 チームで作成したゲームについて紹介いたします。

## 概要

本ゲームは日本ゲーム大賞向けの企画として制作を開始しました。
日本ゲーム大賞は毎年テーマを提示しており、2022 年度のテーマは「感触」でした[^1]。
当時 dokudami がスノーボードにハマっており、雪原をすべる「感触」が感じられるゲームができたらおもしろいのでは、という思い付きで企画いたしました。

## コンセプト

ゲームの世界感としては、電脳世界を少女が音楽とともにボードで滑走するようなものをイメージしていました。
これは、鉢音ちゃんねる[^2]という初音ミクの姿でスノーボードを滑る動画投稿者様の影響を多分に受けております(;´∀ ｀)。
初期の画面構成は以下の画像のような感じで、ワイヤフレーム状の世界を三人称視点で少女を操作してスタートからゴールまでを目指すという構成でした。

{{< gallery>}}
{{< figure link="hatine-anime.gif" caption="鉢音さん" >}}
{{< figure link="concept-image-1.webp" caption="コンセプト1" >}}
{{< figure link="concept-image-2.webp" caption="コンセプト2" >}}
{{< /gallery>}}

## デザイン

デザイナーの方にキャラクターデザイン、3D モデルの制作、UI のデザインをしていただきました。

### キャラクターデザイン

キャラクターは、任天堂の「Splatoon」の 1 号のような小柄ながらもたくましさのある感じのキャラクターに仕上げていただきました。
いろんな色の世界を滑るので、色は白をベースにハイライトで何色にも染まれるようにしてもらいました。

{{< gallery>}}
{{< figure link="character-1.webp" caption="キャラデザ資料１（kk）" >}}
{{< figure link="character-2.webp" caption="キャラデザ資料２（kk）" >}}
{{< figure link="character-3.webp" caption="キャラデザ資料３（kk）" >}}
{{< /gallery>}}

### 3D モデル

キャラクターデザインに引き続き、3D モデルの制作も行っていただきました。
少女自身の容姿や髪、服装はもちろんのこと、ゴーグル、マスク、ニット帽、ボードまで作成してもらいました。

{{< gallery>}}
{{< figure link="character-3d-1.webp" caption="キャラクター3Dモデル（塩味）" >}}
{{< figure link="character-3d-2.webp" caption="キャラクター3Dモデル（塩味）" >}}
{{< figure link="character-3d-3.webp" caption="キャラクター3Dモデル（塩味）" >}}
{{< /gallery>}}

ステージのオブジェクトに関しても作成していただきました。
T2-ファージのようなオブジェクト、DNA のようなオブジェクト、雪の結晶のようなオブジェクト、崩壊したビルのようなオブジェクトが確認できますね 👀。

{{< gallery>}}
{{< figure link="image-4.webp" caption="ステージ配置オブジェクト（fibrin, 塩味）" >}}
{{< figure link="image-6.webp" caption="ステージ３" >}}
{{< figure link="image-5.webp" caption="ステージ４" >}}
{{< /gallery>}}

### UI デザイン

タイトルロゴ、キャラクターの服についているマーク、UI なども作成していただきました。
タイトルは、滑走する少女という意味で GlideGirl というタイトルにし、"G"が 2 つあることを生かしたデザインにしてもらいました。
ステージ選択の UI は、滑らかなアニメーションをさせつつ、サイバーな見た目にしてもらいました。

{{< gallery>}}
{{< figure link="title-anime.gif" caption="タイトル画面" >}}
{{< figure link="ui-anime.gif" caption="ステージ選択画面" >}}
{{< /gallery>}}

## サウンド

本ゲームは 1 曲 1 ステージの構成でステージが存在します。
今回、コンポーザーの方々に計 4 曲を作成していただき、それぞれに対応したステージを実装しました。
各曲には以下の Sound Cloud から聞くことができます。
是非聞いてみください！

{{< soundcloud-playlist 1513300114 >}}

## プログラム

プログラムの実装ももちろん行いました。
特に大変だった箇所としては、コースの設定と滑走動作の実装でした。

コースは曲線のカーブを描かせたかった為、ベジェ曲線でコースを編集できる仕組みを自前実装しました。
設定したベジェ曲線に沿って動的にメッシュが生成されるような仕組みになっています。
（現在では Unity の標準機能でベジェ曲線が扱えるようになったようです[^3]。）

滑走動作は、キャラクターのポーズ → 傾け → 動作のように 1 つずつスノーボーダーの動作を再現していきました。
最終的に Lerp 線形補間を使った滑らかな動作になったと思います。

{{< gallery>}}
{{< figure link="program-3.gif" caption="ベジェ曲線" >}}
{{< figure link="program-1.gif" caption="滑走動作（初期）" >}}
{{< figure link="program-2.gif" caption="滑走動作（最終）" >}}
{{< /gallery>}}

## おわりに

2022 年度に NITMic 部員で作成したゲーム「GlideGirl」について紹介させていただきました。
制作に協力していただいた部員の方々、ありがとうございました。

※ 本ゲームは工大祭 2022 での展示も行いました[^4]。

## Credit

- プログラム
  - dokudami, kuru, hydriod, Kihiro, ハヤシ, Taku-san, たくみ, ショコラ
- 3D デザイナー
  - fibrin, 塩味
- 2D デザイナー
  - kk, fjktkm, kuru
- コンポーザー
  - Mass, shenzel, Thike

[^1]: [日本ゲーム大賞 2022「アマチュア部門」](https://awards.cesa.or.jp/2022/amateur/)
[^2]: [スキージャム勝山が最高すぎるよ鉢音さん！スキー、スノーボードが最高に楽しめるスノーリゾート - YouTube](https://www.youtube.com/watch?v=P0ldXs4R7ho)
[^3]: [Unity 2022 新機能！スプラインを使ってみよう！ - YouTube](https://www.youtube.com/watch?v=5IrKqVnvP6M)
[^4]: [NITMic ゲームセンター 2022 - NITMic](https://nitmic.club.nitech.ac.jp//post/koudaisai-2022/)
