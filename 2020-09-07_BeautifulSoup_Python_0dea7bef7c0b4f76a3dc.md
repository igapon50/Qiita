<!--
title:   Python+BeautifulSoupでスクレイピングしてみる
tags:    BeautifulSoup,Python,スクレイピング
id:      0dea7bef7c0b4f76a3dc
private: false
-->
# 目的
Python+BeautifulSoupによるスクレイピングを学習する。

# 背景
ウェブサイトから画像だけダウンロードしたいと思い、
スクレイピング出来たら簡単にできそうだと安易に考えたが、これが思いのほか大変だったので、とりあえずやったことを書く。

# 概要
スクレイピングの学習がしたかったので、画像をダウンロードする部分は作らない。そこはダウンローダーのフリーソフトIrvineを使用する。
また、ダウンロードした画像ファイルは、ナンバリングしたファイル名につけ直して、Zipファイルにまとめる。フローは以下の通り。

1. 作成ツールでウェブサイトから画像のURLリストを作り、クリップボードにコピーする
1. Irvineにペーストしてダウンロードする
1. 作成ツールでファイル名を付けなおして、zipファイルに圧縮する

ところでIrvineの機能をちゃんと使えば、こんなの作らなくても全部できるだろ！とか言わないように。あくまでも目的はスクレイピングの学習なので。

# 環境と設定
Windows10で実施した。
chocolateyを使っているならpython3のインストールは、管理者権限でcmdまたはWindows PowerShellを起動して以下のコマンドを実行する。

```dos
> choco install python
```

途中選択肢が出る場合は、すべてy + Enterする。
インストールが終わったら、cmdまたはPowerShellを開きなおして、以下のコマンドを実行する。

```dos
> pip install requests
> pip install bs4
> pip install pyperclip
```
[Git](https://github.com/igapon50/training/releases/tag/1.1.0)からSource code(zip)をダウンロードして、展開する。
展開したパスを「Git/traning/」とする。
[Irvine](http://hp.vector.co.jp/authors/VA024591/)をダウンロード＆インストールして起動する。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/b5c17928-b9d9-98c7-b45b-5939f9e74d94.png)
デフォルトフォルダに「folder01」フォルダを新規作成する。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/6a2aab20-a3e0-50e1-bb7b-1803d61f421c.png)
新規作成するフォルダ「folder01」は、スクリプト「HTML2imglist.py」
のあるパスとする。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/e553a28c-26ba-999a-c604-f0c22122a625.png)
「folder01」の右クリックコンテキストメニューから「フォルダ設定」を選んで、後から変更できる。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/95192769-1e84-a312-2177-ee073c776ea4.png)
Irvineに「folder01」が追加された。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/2246b26f-01b0-f73a-29fb-56542a036e23.png)
メニューから「ツール」-「オプション設定」を選ぶ。
タブ「クリップボード」を開き、チェックボックス「クリップボードから直接登録する」をONにする。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/c409362f-5c41-a93d-7828-710b15174cb2.png)
OKボタンをクリックして閉じる。
他にメニューから「管理」-「クリップボード監視」を選択してONにしなければならないかもしれない。私の手元ではONでもOFFでも動いた。

# 使い方
地元石川県のオープンデータがある[ウェブサイト](https://www.hot-ishikawa.jp/photo/)をターゲットとして説明する。

以下のような名勝のサムネイル画像をターゲットとする。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/c0be049f-e83c-6dc3-d332-32d8c8f35edd.png)

「Git/traning」パスでコマンドプロンプト(cmd)を起動したとすると、パスを「Git/traning/python/Web_scraping」に移動する。
ダウンロードしたい画像があるウェブサイトのURLを引数に指定して、スクリプトを実行する。

```dos
> cd .\python\Web_scraping
> python Html2imglist.py https://www.hot-ishikawa.jp/photo/
```
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/a72824a1-3464-1a71-75cd-7a8f11c1c74b.png)
するとタイトルと、画像のURLリストがクリップボードにコピーされる。
Irvineを起動して、「folder01」にペーストするとダウンロードが開始されるので完了するまで待つ。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/72cac8f0-2079-5e4f-6efd-5f42f0a45853.png)
コマンドプロンプトに戻り何かキーを押せば、ダウンロードした画像ファイルを、ナンバリングしたファイル名につけ直し、Zipファイルにまとめる。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/439803d4-3f14-4105-03dc-845e65b41557.png)
↑「folder01.zip」が出来ている。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/96799502-04e4-651f-9ff8-64184eba7e49.png)
さらに何かキーを押せば、「folder01」フォルダを空にする。
「folder01.zip」をビューアソフト、例えば[Image Viewer](http://andantissimo.jp/)にドラッグ＆ドロップしてみると。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/86f15f34-033c-714f-d559-11ba2d95a6b8.png)
無事表示された。
[Image Viewer](http://andantissimo.jp/)は、→キーで次のスライド、←キーで前のスライドに切り替わる。

# スクレイピング

ターゲットにしたサイト「[写真素材ダウンロード｜ほっと石川旅ねっと](https://www.hot-ishikawa.jp/photo/)」のソースコードを表示して、titleタグを確認する。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/82724a51-510a-0b24-d40c-5c481e1cc025.png)
これをCSSセレクタで表現すると「html head title」となる。
また、画像ファイルまでのタグ構造を確認する。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/5589c9e3-a305-f50f-823f-30ac8f41aec6.png)
ダウンロードしたいファイルは以下のsrc属性である。

```html
475行目：<img class="img-responsive" src="/photo/thumbnail/749/trim/1/1?v=0ca07195022078860363c009b75962f59c80bde5" alt="兼六園">
～
486行目：<img class="img-responsive" src="/photo/thumbnail/740/trim/1/1?v=f4145f658b274299f83a6038ef58f9b8d0cb5ac1" alt="金沢駅">
```
ここまでのタグの連なりは以下の通りになっている。

```html
<html>
    <body>
        ～
        <div class="photoItems">
            <ul>
                <li>
                    <div class="photoItem">
                        <a>
                            <img src="対象の画像１枚目">
                        </a>
                    </div>
                 </li>
                <li>
                    <div class="photoItem">
                        <a>
                            <img src="対象の画像２枚目">
                        </a>
                    </div>
                 </li>
```
これらをCSSセレクタで表現すると「html body div .photoItems ul li div .photoItem a img」となる。
少し省略して「html body div .photoItem img」とする。

この画像ファイルの記載のされ方は、ウェブサイトによって変わるので、HTML2imglist.pyファイルの以下の変数で指定できるようにした。

```python
50行目：title_css_select = 'html head title'
51行目：img_css_select = 'html body div .photoItem img'
52行目：img_attr = 'src'
```

# おわりに
CSSセレクタがスクレイピングそのものなんだなと思った。
そう考えると、この記事はスクレイピングを何も学習していないことになるけど、気のせいに違いない。

# 関連リンク
ターゲットにしたサイト
- [写真素材ダウンロード｜ほっと石川旅ねっと](https://www.hot-ishikawa.jp/photo/)

参考
- [note.nkmk.me](https://note.nkmk.me/)
- [Beautiful Soup 4.2.0 Doc. 日本語訳](http://kondou.com/BS4/#)
- [MDN web docs ja - element.querySelectorAll](https://developer.mozilla.org/ja/docs/Web/API/Element/querySelectorAll)
- [Python Tips 山本隆さんのサイト](https://www.gesource.jp/programming/python/index.html)
- [Irvine Help](http://hp.vector.co.jp/authors/VA024591/doc/manual.html)
- [Uikipedia Irvine](https://romaji.fandom.com/ja/wiki/Irvine)
- [Image Viewer](http://andantissimo.jp/)