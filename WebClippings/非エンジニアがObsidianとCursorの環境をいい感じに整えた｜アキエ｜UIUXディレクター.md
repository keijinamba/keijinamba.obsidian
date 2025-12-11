---
type: "web-clipping"
title: "非エンジニアがObsidianとCursorの環境をいい感じに整えた｜アキエ｜UI/UXディレクター"
source: "https://note.com/akienakai/n/nad947525d548"
author:
  - "[[アキエ｜UI/UXディレクター]]"
published: 2025-05-13
created: 2025-12-11
description: "こんにちは。外資系コンサルのデザイン部門にてUI/UXディレクターをしているアキエ（@__aknaka__）です。  最近、𝕏で話題になっている「Obsidian in Cursor」（AIコードエディタ\"Cursor\"でノート管理アプリ\"Obsidian\"を使いこなす）を始めました！  初心者ながら環境をあれこれ整えていく中で色々と詰まったので、その記録を残そうと思います。 非エンジニアでObsidianとCursorにチャレンジしてみたい方の参考になれれば嬉しいです。   【2025/7/12】Kindle Highlightsプラグインと、Obsidian Web Clipper"
image: "https://assets.st-note.com/production/uploads/images/189648443/rectangle_large_type_2_29ff99560c50416be8f9043ea49e9cfd.png?fit=bounds&quality=85&width=1280"
tags:
  - "WebClipping"
in:
  - "[[MOC/WebClipping]]"
---

![見出し画像](https://assets.st-note.com/production/uploads/images/189648443/rectangle_large_type_2_29ff99560c50416be8f9043ea49e9cfd.png?width=1280)

## 非エンジニアがObsidianとCursorの環境をいい感じに整えた

[アキエ｜UI/UXディレクター](https://note.com/akienakai)

こんにちは。外資系コンサルのデザイン部門にてUI/UXディレクターをしているアキエ（@\_\_aknaka\_\_）です。

最近、𝕏で話題になっている **「Obsidian in Cursor」** （AIコードエディタ"Cursor"でノート管理アプリ"Obsidian"を使いこなす）を始めました！

初心者ながら環境をあれこれ整えていく中で色々と詰まったので、その記録を残そうと思います。  
非エンジニアでObsidianとCursorにチャレンジしてみたい方の参考になれれば嬉しいです。

> 【2025/7/12】Kindle Highlightsプラグインと、Obsidian Web Clipperプラグインのテンプレートについて追記しました。

## この記事の目標

本記事では、以下を達成するための手順を説明します！

- CursorでObsidianを編集できるようになる
- Kindle書籍の知識をObsidianに蓄えられるようにする
- iPhoneとPCでObsidianを同期し、iPhoneでメモを取れるようにする（同期にはGithubを利用）
- iPhoneとPC両方から、Web上のコンテンツをObsidianに蓄えられるようにする

**この記事の対象者：**

- これからCursorでObsidianを始めたい方（特に非エンジニアの方向け）
- CursorとObsidianをセットアップし、メモ管理の環境をいい感じに整えたい方
- スマホとPCの同期方法に悩んでる方 → わたし自身ここが一番大変でした

**この記事で述べていないこと：**

- メモ管理の具体的な運用方法、ノウハウ
- Cursorを使った効率的な記事作成方法

なお、Obsidian in Cursorのメモ管理の仕組みやツールなどは以下の記事をかなり参考にさせていただいています🙇♀️

## ObsidianとCursorについて

まずはObsidianとCursorについて簡単に紹介します！

**Cursorとは？**

[**Cursor: AIで最高のコーディング体験** *驚異的な生産性を引き出すために設計されたCursorは、AIとともにコードを書く最良の方法です。* *www.cursor.com*](https://www.cursor.com/ja)

AIと一緒にコーディングができるコードエディタです。  
画面中央はファイルスペース、右側にチャットスペースがあり、AIにさまざまな作業をお願いしながら、一緒に共同作業する感覚でファイルを作成することができます。  
最近では、 **記事を生成・編集するツール** としても注目を集めています。

![画像](https://assets.st-note.com/img/1746997650-GBIUfdFzeVvM8oHiYb3WQ7sn.png?width=1200)

**Obsidianについて**

[**Obsidian - Sharpen your thinking** *The free and flexible app for your private thoughts.**obsidian.md*](https://obsidian.md/)

ローカル端末で動作する、Markdown形式のノートアプリ（ドキュメントエディタ）です。現在、Notionの代替ツールとして注目を集めています。なぜ注目されているかというと、

- Markdown形式（.md）がAIに読み取られやすい
- Notionは階層が深くなりがちだが、Obsidianはフラットでタグベースの管理のため、AIが読み取りやすい
- 拡張機能が充実しており、色々なアプリと連携したり、カスタマイズしやすい

このようにAIフレンドリーなエディタのため、これからのAIネイティブな知的生産や仕事にとても親和性がありそうです。

エディタと聞くとちょっととっつきにくいイメージがありますが、Notionもノート執筆の記法はMarkdownに由来しており、UIもまあまあ似ているので同じような操作感で使用することができます。

![画像](https://assets.st-note.com/img/1747002835-8PijONQM721xnWVlgHU3RXJ9.png?width=1200)

## なぜObsidian in Cursorを始めたのか

普段、Webサイトやアプリのリニューアル案件の画面設計や制作進行をしている非エンジニアのわたしが、Obsidian in Cursorを始めようと思ったのは以下の理由があります。

- **育児と仕事がある中で日々の内省・タスク管理・インプットとアウトプットを効率化したい**
- **生成AIを使うことに慣れていきたい**
- **職場でNotionが使えない**

１つ目について。わたしは現在育休中でして、この期間を活用して色々な情報をインプットし勉強したり、𝕏やnoteで発信を始めたりしています。  
これが職場復帰するとなると、育児もある中で時間的になかなか両立できないのではと危惧しています。 **「隙間時間でも手軽にメモして、メモの内容を元にアウトプットする」** というサイクルを効率化するための手段として、Obsidian in Cursorに期待しています。

２つ目について。生成AIツールを活用して生産性を上げたり、何か生み出したりするには **「積極的に使って慣れる」** ことが重要だと思っています。  
今やっているディレクション業務において、生成AIをどのように活用するのか、正直まだ見えてないです。  
ですが、まずは日常生活で生成AIを積極的に取り入れて使うのに慣れてきたら、仕事での活用方法もおのずと見えてくるのではないかと思っています。

３つ目についてはクリティカルですね。仕事をしている中で出てきた反省点はメモ管理ツールにすぐにメモするようにしているのですが、後から振り返るためにスマホやプライベートPCからいつでも見れるようにしたいです。そのためにメモ管理ツールは **会社PC、プライベートPC、スマホ（わたしの場合iPhone）で同期されアクセスできることが理想** です。  
実は会社PCではNotionの使用が禁止されており、アプリダウンロードができません。Web版にもアクセスすらできなくて代替ツールを探していました。大きな声で言えないのですが、ObsidianとCursorは比較的最近出てきた印象があるので、会社PCでもダウンロードできるのではないかと勘繰っています。（どうか使えますように・・・）

## セットアップの流れ

前置きが長くなってしまいましたが、本記事では以下のステップで説明してきます。ダウンロードから始まって、各ツールや拡張機能のセットアップをしていく流れです。

**ステップ1: ObsidianとCursorのセットアップ  
**Obsidianのインストールと基本的な設定を行い、PCでCursorを用いて編集できる環境を整えます。

**ステップ2: Kindle Highlightの設定  
**Kindleで読んだ本のハイライトをObsidianに自動同期する環境を整えます。

**ステップ3: LINE Note Syncの設定  
**LINEを経由して、スマホからObsidianに手軽にメモを取れる環境を構築します。

**ステップ4: Githubによる同期環境の構築  
**スマホとPC間でノートを同期するためのGit環境を構築します。

**ステップ5：Web上の情報をストックする**  
スマホとPC両方から、Web上のコンテンツをObsidianにストックできるようにします。

"Github"というワードも出てきて「うっ」となるかもしれませんが大丈夫です。 **ChatGPTやCursorの助けを借りながら設定するやり方** もお伝えします。  
もし難しい場合は、ステップ１〜５のできるところだけでもやってみてください。

## ステップ1: ObsidianとCursorのセットアップ

まずPC側の環境を整えます。

### Obsidianのインストール

1. [Obsidian公式サイト](https://obsidian.md/) からダウンロードする  
	→ アカウントはObsidian Sync（有料）を利用するために必要なものなので、アカウント作成しなくてOKです！
2. 画面指示に従ってインストール手順に従ってセットアップする
3. Obsidanを起動し、「保管庫を新規作成する」の「作成」をクリックして保管庫（Vault）を作成する。名前は適当でOKです。（わたしは「MyVault」と設定しています）  
	→ Vaultとは、ファイルやフォルダを作るための格納場所を指します。

> **ポイント💡**  
> 保管庫の場所は、後ほどCursorで開くのでわかりやすい場所に設定しておきましょう！

![画像](https://assets.st-note.com/img/1747001189-qDR56rPBpSOC1bmvKUdWMTu8.png?width=1200)

ここまでで一旦、Obsidianのセットアップは完了です！続いて、CursorでObsidianの環境を整えていきます。

### Obsidian in Cursorのセットアップ

1. [Cursor公式サイト](https://cursor.sh/) からダウンロードする
2. 画面指示に従ってインストール手順に従ってセットアップする
3. Cursorを起動し、先ほど作ったObsidianのVaultフォルダを開く
4. Vault内にフォルダを作成する  
	→Cursorを使ってフォルダを一気に作るのがおすすめです！（後述）

**フォルダ構成**

```python
MyVault/
└── Zettelkasten/        
    ├── 01_IndexNote/     # インデックスノート
    ├── 02_Rule/  # 運用ルールに関するノート（まだ使っていない）
    ├── 10_PermanentNote/ # 最上位ノート
    ├── 20_LiteratureNote/ # 文献ノート
    └── 30_FeelingNote/   # メモ・アイデアノート
```

わたしは先の [引用記事](https://note.com/shotovim/n/n5833578984bf) を参考に、まずは簡易的にフォルダを構成しています。 **「Zettelkasten（ツェッテルカステン）」** というメモ管理術がベースになっていて、フラットにファイル管理し、メモ同士をリンクすることができるObsidianと相性が良さそうです。（Zettelkastenも流行ってますね・・・）

  
構成を簡単に説明すると以下の通りです。まずはメモやアウトプットを溜めて、慣れていったらルールを入れたフォルダ（02\_Rule）も作り、効率的に運用していきたいです。

- 30\_FeelingNote：メモやひらめき、感想を手軽に残していくフォルダ。後述するLINEを用いたメモもこちらにインポートしています。
- 20\_LiteratureNote：Webからの情報や、Kindle書籍のハイライトを溜めていくフォルダ。
- 10\_Parmanentnote：20\_LiteratureNoteや30\_FeelingNoteをもとに、自分の言葉でアウトプットしたものを溜めるフォルダ。noteの下書きもここに入れています。

**AIエディタを使ってフォルダを作成する**

手始めに、Cursorを使ってフォルダを作成することをやってみます。  
すぐに実行するのではなく、進めて良いか都度確認してくれ、「Run」を押すとコードが実行されるので安心感があります。  
以下のプロンプトを入力すると一気にフォルダが作られて、しかもZettelkastenの運用方法まで教えてくれました。

> **プロンプト例：**  
> Zettelkastenディレクトリ内に「10\_Parmanentnote」「20\_LiteratureNote」「30\_FeelingNote」を作成して。

![画像](https://assets.st-note.com/img/1747003969-dwJMuZPvEa5Il8niCXSo7gBx.png?width=1200)

これで、メモを溜めていく準備が整いました。

## ステップ2: Kindleハイライトの抽出

Kindle書籍ハイライトを、20\_LiteratureNoteにインポートする設定を行います。

### Kindle Highlightプラグインの設定

1. Obsidianの設定画面（左下の歯車アイコン）を開く
2. 「オプション」→「コミュニティプラグイン」→「閲覧」をクリック
3. 検索バーで「Kindle Highlights」と入力して検索
4. 「Kindle Highlights」プラグインをインストールして有効化
5. プラグインの設定で「Amazon Region」を 「Japan (amazon.co.jp)」 に設定し、同期先フォルダを指定します。

これで設定完了です。「Sync on Startup」がデフォルトでONになっており、アプリ起動時にハイライトがファイルとして自動取得されます。  
手動で同期するには、サイドバーから「Sync your Kindle highlights」（下画像の赤枠部分）をクリックします。

![画像](https://assets.st-note.com/img/1747005463-b9wxOj51u3nJfdIDhCRP8oKp.png?width=1200)

### 【2025.07.12追記】テンプレートの編集

デフォルトだと、保存されたファイル名が「著者名ータイトル」となってしまいわかりにくかったので、テンプレートを編集することで見やすく調整しました。

以下の記事で紹介されている松濤Vimmerさんのテンプレートを活用していますが、一つだけ、tagsの下にimage:{{imageUrl}}プロパティを追加しています。

↓File template

```php
---
tags: kindle
image: {{imageUrl}}
---

# {{title}}
{% if imageUrl %}![|300]({{ imageUrl | replace("._SY160", "") }}){% endif %}
## Metadata
{% trim %}
{% if authorUrl %}
* Author: [{{author}}]({{authorUrl}})
{% elif author %}
* Author: [[{{author}}]]
{% endif %}
{% if asin %}* ASIN: {{asin}}{% endif %}
{% if isbn %}* ISBN: {{isbn}}{% endif %}
{% if pages %}* Pages: {{pages}}{% endif %}
{% if publication %}* Publication: {{publication}}{% endif %}
{% if publisher %}* Publisher: {{publisher}}{% endif %}
{% if url %}* Reference: {{url}}{% endif %}
{% if appLink %}* [Kindle link]({{appLink}}){% endif %}
{% endtrim %}

## Highlights
{{highlights}}
```

こうすることで、後日インデックスノートでギャラリーライクに表示させることができました。

![画像](https://assets.st-note.com/img/1752298903-ndz8QSpiIMcT5320f4wugyt9.png?width=1200)

IndexNoteフォルダに「Kindle Index」のノートを追加しました

Notionライクに表示させる方法は、はたまた松濤Vimmerさんの方法を使わせてもらってます🙏

  

## ステップ3: LINE Note Syncの設定

わたしは、iPhoneからのメモを取るのに以下記事で紹介されているLINE Note Syncを用いています。理由は、「Obsidianアプリを開いて記事を追加して・・・」だと手間がかかるため。  
LINEだとトークをピン留めしておけば、いつでもすぐにひらめきや感じたことをその場でメモすることができます。

わたしが追加で説明することは少ないため、セットアップ手順含め以下記事を参照ください🙇♀️

LINEでメモをとり、送信するだけで自動的にObsidianに同期されます。設定も簡単でした！このような便利なアプリを作ってくれる方がいてありがたい限りです。

  

## ステップ4: Githubによる同期環境の構築

続いて、iPhoneとMacでデータを同期するための環境を整えていきます。  
同期するためのツールは **「Github」** を用います。  
iCloudでの同期も考えたのですが、iCloudの容量を逼迫するためやめました。また、Githubを使っていても容量の大きい画像の保存すると問題が発止するらしいのですが、AWS S3ストレージを使うなどはまだやっていないです。問題が発生したらそのとき考えます笑

### PCでのObsidian同期設定

以下の記事に従って設定します。事前準備で詰まることが多いのですが、大丈夫です。 **ChatGPTとCursorがあれば乗り切れます。**

**１. 事前準備：GitのインストールとGithubのリポジトリの用意**  
Gitプラグインのインストールには、記事にもあるように以下の事前準備が必要です。このうち、 **（１）と（２）はChatGPTを、（３）はCursorを使う** と設定がスムーズです。

（１）PCにGitをインストールする  
（２）Obsidian 用のリポジトリを Github 上に用意する  
（３）VaultをGit管理にする

まずはChatGPTを開き、以下のプロンプトを入力します。

> **プロンプト例：**  
> ObsidianとGithubを同期させたいのですが、どうやれば良いですか？Gitに詳しくないため、コマンドは意味も合わせてわかりやすく教えてください。

そうすると、以下のようにGithubのセットアップの仕方からローカルのGit環境の構築方法まで、ステップバイステップで教えてくれます。  
このうち、 **「ステップ1. Githubのリポジトリ作成」「ステップ2. PCへのGitインストール」** は教えてくれたとおりに進めましょう。

![画像](https://assets.st-note.com/img/1747013005-xlInDjBRaX7OrycMJpL329Vs.png?width=1200)

わたしの場合、Githubの使用経験があるためアカウントの設定方法は省いてくれました。（メモリすごい・・・！）

> **ポイント💡**  
> \- 個人的なメモも多いため、Githubのリポジトリは **PublicではなくPrivateに設定する** のがおすすめです。  
> \- 何かエラーが出たら、スクショと一緒にChatGPTに解決方法を聞いてみましょう。大体これで解決できるはず。

**2.事前準備：VaultをGit管理下にする  
**ステップ3くらいでChatGPTがVaultをGit管理にする手順を教えてくれるのですが、ターミナルにコマンドを打つやり方なので、非エンジニアにとってはなかなかハードルが高いのではないでしょうか。

![画像](https://assets.st-note.com/img/1747013964-fv48XwKB1lPdEmgWxrbijaSy.png?width=1200)

**そんな時はCursorです。  
**以下のような簡単なプロンプトで、AIがGitの環境構築を自動でやってくれます。

> **プロンプト例：  
> **このフォルダをGit管理下にしたい。

このように、Gitの初期化から.gitignoreファイル（同期するときに無視して欲しいファイルの設定情報）の作成など、必要なセットアップをすべて行なってくれます。本当にすごい。

![画像](https://assets.st-note.com/img/1747014617-yqcskrWR0jig1oYHuf4eaFO2.png?width=1200)

**3.Gitプラグインのインストール  
**あとは、 [先の記事の手順](https://note.com/devlive/n/n5d22e9e74641) に従い、プラグインをインストールしてGithubにVault内のファイルをコミット・プッシュ（情報をGithub上にアップロード）できれば完了です！

**＜わたしのケース＞**  
わたしのGitの設定はデフォルトから変えていません。

![画像](https://assets.st-note.com/img/1747030654-bzn4gCF3hfryWxsJGj6AuqET.png?width=1200)

「Split automatic commit and push」で自動コミット・プッシュの設定が可能なのですが、内容ができていない段階で勝手にGithubにアップロードされるのが嫌だったので、手動でコミット・プッシュするようにしています。  
端末間データが競合しないよう、 **PCやアプリケーションを閉じる段階で必ずコミット・プッシュする** ようにします。

ちなみに手動でコミット・プッシュするときは、画面右上の「Source Control」をクリック→一番左の「Commit-and-Sync」を押すことで簡単に実行できます。

![画像](https://assets.st-note.com/img/1747015510-gGaw1eo7LUvPQ9p0WScj3Xtz.png?width=1200)

なお、プッシュする際にエラーが出たのですが、ChatGPTにエラー内容を貼り付け、解決策の通りに進めて乗り切りました。  
認証まわりは個人の環境に依存しそうなので、解説は省略させていただきます🙏

![画像](https://assets.st-note.com/img/1747088641-CbKseqQ7TwFd6h2rxyNgfWUZ.png?width=1200)

### iPhoneでのObsidian同期設定

あと一息です。  
以下の記事の手順でiPhoneにObsidianアプリをインストールし、Gitプラグインで同期設定をします。（説明に他記事をちょくちょく引用していてすみません・・・でも、とてもわかりやすいのでこちらを参照した方が効率的です！）

[**スマホのObsidianをGitで同期(2024.11)** *zenn.dev*](https://zenn.dev/ishikawa096/articles/158246fc5a5d62)

これで、iPhoneとPCでの同期環境の構築は完了です！！

### 同期の手順

ようやく、iPhoneとPC両方から情報をストックする準備ができました。最後に、iPhoneとPCでメモを取り、同期する流れをまとめておきます。

1. iPhone（またはPC版）のObsidianでメモを取る
2. 変更をGithubにコミット・プッシュする（自動でプッシュする設定の場合は不要）
3. PC版（またはiPhone）のObsidianを開くと変更が自動でPullされる。手動で行う場合はGitプラグインの「↓ボタン」を押す
![画像](https://assets.st-note.com/img/1747016238-yEe9Ab6Wl2YkSmo0wXpBidqF.png?width=1200)

PC版のGitプラグインの画面

![画像](https://assets.st-note.com/img/1747036682-uoSxhPUWvDZdMmbsKaplLQYC.png?width=1200)

スマホの場合、右から左にスライドするとプラグインの画面が出てきて、下の方にボタンが並んでいる

## ステップ5：Web上の情報をストックする

ブラウザの拡張機能を使って、Web上の気になる記事をスマホとPC双方からObsidianにストックできるようにします。

**1.PCでWebページをストックする  
**以下のChrome拡張機能をインストールします。

[**Obsidian Web Clipper** *Highlight and capture web pages in your favorite browser. Sav* *obsidian.md*](https://obsidian.md/clipper)

使い方は簡単で、インストール後にChromeのタブからObsidianのアイコンをクリックして保存するだけ。  
設定画面の **「デフォルト」→「ノートの場所」** で、特定のフォルダ（ここでは20\_LiteratureNote）を保存先として指定することができます。

![画像](https://assets.st-note.com/img/1747031455-iEdHnPvG0FBLsUzoNhuSl4y7.png?width=1200)

**【2025.07.12追記】テンプレートの編集**  
Web clipperの方も、後日インデックスノートでギャラリーライクに表示させるために、プロパティを追加しました。設定しておかないとサムネが表示されずクリップし直したくなるので、最初に設定しておきましょう🙇♀️

左メニューの「デフォルト」をクリック→「＋プロパティを追加」をクリックし、以下のように **image:{{image}}** を追加します。

![画像](https://assets.st-note.com/img/1752299418-1vFZzkOcfiCQWRI7MXlLthbw.png?width=1200)

**2.スマホでWebページをストックする**  
スマホの場合、Chromeの拡張機能が使えないので（泣く泣く）Safariを使っています。（このためにiPhoneのデフォルトのブラウザをSafariに変更しました・・・）  
以下のアプリをインストールします。

[**Obsidian Web Clipper** *Bring the web to your personal knowledge base. Save content* *apps.apple.com*](https://apps.apple.com/us/app/obsidian-web-clipper/id6720708363?platform=iphone)

インストール後、SafariでWebページを開き、下部のURL入力タブ左の拡張機能アイコンをクリック→「Obsidian Web Clipper」をタップして同様にページを保存することができます。  
ここでも、PC版と同様に **事前に保存先フォルダを設定する** ことを忘れずに。

![画像](https://assets.st-note.com/img/1747031326-bqZ9SJjGyAzxLcUN2dIWheDa.png?width=1200)

**3.ObsidianでGithubに同期する**  
記事を保存したあと、ステップ4で手動同期の設定にしている場合は、前述のGitプラグインを用いてコミット・プッシュします。  
自動同期の設定になっている場合は自動でプッシュされるので何もしなくてOKです。

  

## 今後やりたいこと

お疲れ様でした！  
これでようやく、ObsidianとCursorを使って、スマホとPC両方からメモをストックしていく準備が整いました。  
正直まだまだ機能は使いこなせてないのですが、まずはメモや学びを蓄積していくところが第一歩かなと思ってます。

今後やりたいことはこんな感じです。

- タグやリンクを使ってメモ（LiteratureNote）同士を繋げていき情報を探しやすくする＆知識を関連づけていく
- Cursorを使って効率的にアウトプット（note記事など）を作る

今回、実験的にこの記事の下書きをCursorで作ってみてもらいましたが、本当に叩きという感じで、9割くらい書き直しました笑

ネット上にはさまざまな活用方法が紹介されてるので、わたしもそこから学びつつ、デザイン業務や普段の仕事、日常生活でも活用できるやり方があればまたシェアしていきたいと思います。

[**内部リンク - Obsidian 日本語ヘルプ - Obsidian Publish** *内部リンクはナレッジネットワークの根幹となるものです。 ファイルへのリンク 内部リンクを作成するには \[\[ と入力するだけ* *publish.obsidian.md*](https://publish.obsidian.md/help-ja/%E3%82%AC%E3%82%A4%E3%83%89/%E5%86%85%E9%83%A8%E3%83%AA%E3%83%B3%E3%82%AF)

[**Cursorを使った文章執筆は、AIファーストな環境整備から始まる - 本しゃぶり** *Markdown形式での情報一元管理と音声入力、そしてCursorというAIエディタの組み合わせで、執筆環境が一変した体験* *honeshabri.hatenablog.com*](https://honeshabri.hatenablog.com/entry/cursor_markdown_ecosystem)

ここまで読んでくださりありがとうございました！  
気に入っていただけたらスキ・コメント・フォロー・シェア・サポートしていただけると励みになります！

X（Twitter）では普段、UI/UX設計やディレクターとしてのキャリアのことを発信しているので、お気軽にフォローいただけると嬉しいです◎  
[https://x.com/\_\_aknaka\_\_](https://x.com/__aknaka__)

[![買うたび 抽選 ※条件・上限あり ＼note クリエイター感謝祭ポイントバックキャンペーン／最大全額もどってくる！ 12.1 月〜1.14 水 まで](https://assets.st-note.com/poc-image/manual/production/20271127_pointback_note_detail.jpg?width=620&dpr=2)](https://note.com/topic/campaign)

非エンジニアがObsidianとCursorの環境をいい感じに整えた｜アキエ｜UI/UXディレクター

---