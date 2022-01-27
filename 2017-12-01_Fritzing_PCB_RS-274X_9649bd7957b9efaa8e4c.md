<!--
title:   回路初心者がFritzingで基板発注してみた
tags:    Fritzing,PCB,RS-274X,ガーバーデータ,基板
id:      9649bd7957b9efaa8e4c
private: false
-->
## 目的
この文書はfritzingを用いて作成したブレッドボード図から[seed(FusionPCB)](https://www.fusionpcb.jp/index.html)に基板発注するまでの手順をまとめます。
## 背景
私は電子回路初心者です。ブレッドボードで動作を検証した後に、ハンダ付けする際に、部品を付け間違えたり、どんな回路だったかわからなくなることが良くあります。そこでブレッドボードからすぐにハンダ付けせずに、ブレッドボード図を書き起こして、その図と比較しながらハンダ付けするようにしています。Fritzingはブレッドボード図が簡単に書けるので良く利用していますが、このアプリにはプリント基板図作成機能もあるので、基板発注できればミスがもっと減ると考えました。Fritzingからseeed(FusionPCB)に発注した人がいるとの噂は聞いていたのですが、詳しい方法がわからなかったので、取り組んだ内容をメモします。
記事作成途中に[こちら](http://blog.shuto.info/2017/06/26/fritzing%E3%81%A7%E5%9F%BA%E6%9D%BF%E3%82%92%E4%BD%9C%E3%81%A3%E3%81%A6%E3%80%81%E7%99%BA%E6%B3%A8%E3%81%99%E3%82%8B/)のブログを見つけたので重複する部分はなるべく割愛しました。

## 環境
Windows10
chocolateyを使っているならFritzingのインストールは、コマンドプロンプトを管理者権限で起動して以下のコマンドを実行します。
`choco install fritzing`
バージョンは以下の通りです。
`choco list fritzing`
Fritzing 0.9.3
`choco`
Chocolatey v0.10.8
(chocolateyのインストールは[この記事](https://qiita.com/NaoyaOura/items/1081884068fe3ea79570)を参照)

## ブレッドボード図からプリント基板図作成
### ボードサイズについて
10×10cmまでは追加料金無しなので、これより小さく作りました。
### Layerについて
2層基板までは追加料金無しなので、2層基板で作りました。
### シルク印刷について
どうやって作ったかわかるように、情報(例えば使用アプリ)を書いておきました。
いくら書いても追加料金無しなので、部品説明(端子の電圧や極性)も両面に書いておけば良かった。
### スルーホールについて
スルーホールをいくら用意しても追加料金無しなので、余白に予備のスルーホールを用意しておけば良かった。
私は電源端子の周囲に直列に抵抗と並列にコンデンサを付けられるように余分にスルーホールを用意しておきました。
(シールドなら不要ですが)単体の基板を作るなら、四隅に基板固定用のスルーホールを用意すると良いと思います。
### 自動配線について
自動配線なのに、本来接してはならないスルーホールに接する配線が行われました。手作業で配線を少し遠ざけるなど直しました。
### DRC(DesignRulesCheck)について
ガーバーファイル出力前に必ずDRCを実施してエラーを無くしましょう。
前述の「自動配線で本来接してはならないスルーホールに接する配線」がチェックに引っかかりました。
### プリント基板図作成例
注文に使用したFritzingファイルは[arduino_uno.fzz](https://github.com/igapon50/NTigapon/tree/master/arduino/sketch/arduino_wii_Hastler/arduino_uno.fzz)です。
このファイルをFritzingで開き、プリント基板の画面を表示します。
![プリント基板](https://scontent-nrt1-1.xx.fbcdn.net/v/t1.0-9/23319316_807246546122166_8376857850750122556_n.jpg?oh=30f3a130e7d28e52f963fd6308b51ee3&oe=5ACFE4A7)
ウィンドウの下側に表示される「Export for PCB」の緑枠で囲った▼マークをクリックして、「Extended Gerber (RS-274X)...」を選択します。
![image.png](https://qiita-image-store.s3.amazonaws.com/0/201344/9eb292d5-e87c-015e-9efe-75a03345a2eb.png)
エクスポートするフォルダを選びます。
![image.png](https://qiita-image-store.s3.amazonaws.com/0/201344/216aaa4e-4665-e864-0a13-27e99eb99c11.png)
指定したフォルダにファイルが生成されました。
![image.png](https://qiita-image-store.s3.amazonaws.com/0/201344/5cc4eccd-737b-24e8-7a3c-9b6171054064.png)
このままのファイルでは注文に使えないので[seeed(FusionPCB)が指定するファイル名](http://support.seeedstudio.com/knowledgebase/articles/1176532-how-to-generate-gerber-file)に変換します。
[ファイル名変換用のバッチファイル](https://github.com/igapon50/NTigapon/tree/master/arduino/sketch/arduino_wii_Hastler/PCB/convert.bat)を用意したので、このファイルを先ほどのフォルダに入れます。
コマンドプロンプトを起動し、先ほどのフォルダまで移動して、Fritzingファイル名を引数に指定して実行します。
例として、Fritzingファイルがarduino_uno.fzzであれば以下のコマンドとなります。
>.\convert.bat arduino_uno

※convert.batの利用は自己責任でお願いします。正しく発注ができなかったとしても作者は責任を負いません。あしからず。
![image.png](https://qiita-image-store.s3.amazonaws.com/0/201344/7e118d75-fd1d-7281-2455-cae349388563.png)
ファイル名が変換されました。
![image.png](https://qiita-image-store.s3.amazonaws.com/0/201344/bf5e1679-50c5-ccff-9224-3c1a8ff67fde.png)
変換されたファイルを選択して、右クリックメニュー「送る」から「圧縮(zip形式)フォルダー」にします。
![image.png](https://qiita-image-store.s3.amazonaws.com/0/201344/df314356-391b-a75d-1a83-24aa248e93cd.png)

## プリント基板図から発注する方法
プリント基板図から発注する方法は、先に紹介した[こちら](http://blog.shuto.info/2017/06/26/fritzing%E3%81%A7%E5%9F%BA%E6%9D%BF%E3%82%92%E4%BD%9C%E3%81%A3%E3%81%A6%E3%80%81%E7%99%BA%E6%B3%A8%E3%81%99%E3%82%8B/)のブログを参考にどうぞ。

流れとしては、先ほど作成したzipファイルをseeed(FusionPCB)のWebサイトで読み込ませて、条件を設定して、カートに追加して、支払いを確定すれば完了です。
私の場合は配送にDHLを選んで5日で届きました。
![発注完了](https://scontent-nrt1-1.xx.fbcdn.net/v/t1.0-9/23131043_807247499455404_4391534758372532362_n.jpg?oh=6f80a1ec25dd296d1f4653dc5964d4e4&oe=5A942C0A)

## おわりに
基板発注のおかげで作るのがかなり楽になりました。しかし大量に製造するには実装も何とかしたいので、そのうち実装の注文もやってみたい。