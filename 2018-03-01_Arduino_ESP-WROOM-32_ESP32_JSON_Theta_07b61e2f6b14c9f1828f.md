<!--
title:   ESP32でRICOH THETAの静止画を取得する
tags:    Arduino,ESP-WROOM-32,ESP32,JSON,Theta
id:      07b61e2f6b14c9f1828f
private: false
-->
# 目的
ESP32 Dev Module - arduinoIDE からRICOH THETAに無線LAN接続して、静止画を撮影して取得します。

# 背景
最近流行りのESP32 - arduinoIDE環境からRICOH THETA V(またはRICOH THETA S/SC/SC Type HATSUNE MIKU)を制御するコードをイベント向けに作成したので公開します。

# 環境
Windows10
chocolateyを使っているならarduinoIDEのインストールは、管理者権限でプロンプト(cmd)を起動して以下のコマンドを実行します。
`choco install arduino`
バージョンは以下のコマンドで確認できます。
`choco list arduino`
>Chocolatey v0.10.8
arduino 1.8.5 [Approved]
arduinoide 1.6.1 [Approved]

chocolateyのインストールは、[chocolatey 基本情報まとめ](https://qiita.com/NaoyaOura/items/1081884068fe3ea79570)を参考にセットアップしました。

arduinoIDEをダウンロードしてセットアップする場合は、https://www.arduino.cc/ にアクセスしてメニューから[SOFTWARE]を選び、好きなファイル形式を選びます。
![arduinoide.png](https://qiita-image-store.s3.amazonaws.com/0/201344/031cfa3f-0eb7-21cc-9cff-d20564afe1bc.png)

[Just Download]をクリック※気に入ったら寄付も歓迎とのこと
![arduinoidedownload.png](https://qiita-image-store.s3.amazonaws.com/0/201344/61af6034-8d07-e5d6-d11c-bc679ff55147.png)

arduino-ESP32は、[Arduino core for the ESP32 のインストール方法](https://www.mgo-tec.com/arduino-core-esp32-install)の「２．GitHub からArduino core for the ESP32 のZIPファイルをダウンロード」を参考にセットアップしました。

aJsonは、[Gitページ](https://github.com/interactive-matter/aJson)にアクセスして、[Clone or Download]-[Download]をクリックしてダウンロードします。
![arduinoajsonzip.png](https://qiita-image-store.s3.amazonaws.com/0/201344/3bc6a538-d471-2911-322f-08fb33acda1c.png)

次に[スケッチ]-[ライブラリをインクルード]-[.ZIP形式のライブラリをインストール]を選んで、先ほどDownloadしたaJsonのzipファイルを選ぶとライブラリに組み込まれます。
![arduinoajson.png](https://qiita-image-store.s3.amazonaws.com/0/201344/b50928fa-2f23-3d28-aa62-289418fbb5dc.png)

[秋月電子 ESP32 Dev Module](http://akizukidenshi.com/catalog/g/gM-11819/)
![ESP32 1313](https://scontent-nrt1-1.xx.fbcdn.net/v/t1.0-9/25353664_825430677637086_319148672412066416_n.jpg?oh=bae0ac68797513dff1fa5993bc10f6f5&oe=5AE85BCC)

他にも↓この辺が使えました。
[HiLetgo ESP32 ESP-32S NodeMCU開発ボード](https://www.amazon.co.jp/HiLetgo-ESP32-ESP-32S-NodeMCU%E9%96%8B%E7%99%BA%E3%83%9C%E3%83%BC%E3%83%892-4GHz-Bluetooth%E3%83%87%E3%83%A5%E3%82%A2%E3%83%AB%E3%83%A2%E3%83%BC%E3%83%89/dp/B0718T232Z)
[KKHMF ESP32 ESP-32S NodeMCU開発ボード](https://www.amazon.co.jp/KKHMF-ESP-32S-NodeMCU%E9%96%8B%E7%99%BA%E3%83%9C%E3%83%BC%E3%83%892-4GHz-Bluetooth-%E3%83%87%E3%83%A5%E3%82%A2%E3%83%AB%E3%82%B3%E3%82%A2CPU%E4%BD%8E%E6%B6%88%E8%B2%BB%E9%9B%BB%E5%8A%9B/dp/B077ZSPKLZ/ref=pd_sim_328_3?_encoding=UTF8&psc=1&refRID=NDHWP3GD4F5J9YW770KC)

RICOH THETA V [2.00.2](https://theta360.com/ja/support/download/firmware/v/)
![RICOH THETA V](https://scontent-nrt1-1.xx.fbcdn.net/v/t1.0-9/26991595_843634225816731_9195984500380783976_n.jpg?oh=ee734ee9c2c8782c1fd1af02848a50e2&oe=5AE37E0B)

# 関連リンク
ESP32本家
- [ESP32](http://espressif.com/en/products/hardware/esp32/overview)
ESP32参考
- [滴了庵日録](http://d.hatena.ne.jp/licheng/20170428/p1)
- [DEKOのあやしいお部屋。](https://ht-deko.com/arduino/esp-wroom-32.html)
arduinoIDE参考
- [Windows-arduinoIDE環境構築記事](https://www.mgo-tec.com/arduino-core-esp32-install)
- [Mac-arduinoIDE環境構築記事](https://codezine.jp/article/detail/10033)
aJson参考(ArduinoJsonはパースエラーで使えなかったのでaJsonを使用します)
- [ArduinoJsonとaJsonをESP8266で使う際の比較](http://blog.boochow.com/article/426009137.html)
RICOH THETA本家
- [theta360](https://theta360.com/ja/)

# ベースコード

ボードに[ツール]-[ボード:]-[ESP32 Dev Module]を選択します。
![esp32boad.png](https://qiita-image-store.s3.amazonaws.com/0/201344/0f44fc5c-baf6-9488-c790-963bb434a9e3.png)

[ファイル]-[スケッチ例]-[WiFi]-[WiFiClient]がベースコードとなります。
![サンプル.png](https://qiita-image-store.s3.amazonaws.com/0/201344/2b4bbb6f-ea40-7a9e-ff9b-80062faad106.png)

このスケッチ例は、WiFi接続を行い、GET要求を行い、応答を一行ずつシリアルに出力しています。

# コード修正

コード修正したスケッチを[Git](https://github.com/igapon50/arduino-ESP-THETA/tree/master/Hands-on/1st_period/sample01/WiFiClient)に置きました。

zipでダウンロードする場合は[Git](https://github.com/igapon50/arduino-ESP-THETA)から[Clone or Download]-[Download]します。
zipを展開して「\arduino-ESP-THETA-master\Hands-on\1st_period\sample01\WiFiClient\WiFiClient.ino」を開きます。

RICOH THETA Vと接続する例で説明すると、
16行目から19行目にある定義の中で、接続したいRICOH THETAのモデルのみdefineを有効にします。

```c
#define THETA_V
//#define THETA_S
//#define THETA_SC
//#define THETA_SC_TYPE_HATSUNE_MIKU
```
[スマートフォンと無線LANで接続する](https://theta360.com/ja/support/manual/v/content/prepare/prepare_06.html)の手順4を参考に、20行目にあるパスワードを変更します。

```c
#define AP_PASSWORD "00101594"
```

[RICOH THETA S以降の機種で接続から画像取得するまでの参考資料](https://developers.theta360.com/ja/docs/v2.1/api_reference/getting_started.html)に従って、44行目のGettingStarted()で以下の処理を行っています。
1.APIバージョンの指定(THETA S/SC/SC Type HATSUNE MIKUは必要、THETA Vは不要)
2.撮影前状態の取得
~~3.プロパティの取得・設定~~
4.静止画撮影(ついでにアクセスポイント登録も実施しています)
5.ファイル保存確認(撮影後状態の取得のみ実施しています)
6.ファイル取得(サムネイル画像)

標準の画像はマイコンで扱うにはサイズが大きすぎるため、ここではサムネイル画像を取得します。
[THETA V2.1でサムネイル画像を取得する参考資料](https://developers.theta360.com/ja/docs/v2.1/api_reference/)によると、GET時のURLに「?type=thumb」を付加すればサムネイル画像が取得できます。

# コンパイル＆書き込み

ESP32 Dev ModuleとPCを、マイクロUSBケーブルで接続します。


接続するとデバイスマネージャに認識されます。
![device.png](https://qiita-image-store.s3.amazonaws.com/0/201344/4db59c30-c553-fba4-e71f-26581dd51268.png)


arduino-IDEで[ツール]-[シリアルポート]にデバイスマネージャで確認したCOMポートを選びます。
![serialarduino.png](https://qiita-image-store.s3.amazonaws.com/0/201344/2f9c3abc-30df-5d73-933b-1c94fc9fe0de.png)


コンパイル＆書き込みするには[スケッチ]-[マイコンボードに書き込む]を選びます。
![arduinobuild.png](https://qiita-image-store.s3.amazonaws.com/0/201344/1444321c-d4ac-bf5c-06a0-daad238449bb.png)


# 実行

書き込みが完了すると、自動でリセットがかかり動き出します。[スマートフォンと無線LANで接続する](https://theta360.com/ja/support/manual/v/content/prepare/prepare_06.html)の手順No.1と2を実施して接続されるのを待ちましょう。
(以降の確認はarduino-IDEのシリアルモニタでも出来ますが)ここではteratermで確認しました。(ESP32に書き込みを行う際は、通信に失敗するのでシリアルモニタやteratermを閉じておきましょう)
![teraserial.png](https://qiita-image-store.s3.amazonaws.com/0/201344/8045fe06-10f7-c8c3-0515-994e6bd75a9d.png)

closing connectionまで進んだら、ケーブルを切断するなどして一旦停止しましょう。
![tera.png](https://qiita-image-store.s3.amazonaws.com/0/201344/6b5ddab5-5961-cbe4-ed58-73b75449d595.png)

※arduino-IDEのシリアルモニタの場合
![serial.png](https://qiita-image-store.s3.amazonaws.com/0/201344/011d8b52-f33c-a44d-ac32-ce5a82bd9992.png)

# バイナリデータの取得

シリアルコンソール(teraterm)に表示されたバイナリを範囲選択して、メニューより[編集]-[コピー]します。
![teracopy.png](https://qiita-image-store.s3.amazonaws.com/0/201344/d20767ad-b9aa-7600-aa58-4968e1d26926.png)

PCにバイナリエディタが入っていなければ、ブラウザで以下のリンクを開きます。
https://hexed.it/

0アドレスを選択してショートカットキー(Control-v)でペーストします。
![binaryeditor.png](https://qiita-image-store.s3.amazonaws.com/0/201344/0f819c76-99b7-84f7-615b-db76ba9ed60a.png)

すると、挿入方法を選択する画面がポップアップするので以下の通り選びます。
![binaryeditorpeast.png](https://qiita-image-store.s3.amazonaws.com/0/201344/02f0acc0-d232-ff26-b16b-e71f467331cb.png)

このバイナリデータにはGETリクエストの応答も含まれているので、FFD8までを範囲選択して削除します。
※JPGファイルはFFD8で始まり、FFD9で終わります。
![binaryeditorcut.png](https://qiita-image-store.s3.amazonaws.com/0/201344/de751b20-905d-574b-2349-11da5d4bcdae.png)

Exportボタンをクリックするとファイル「-Untitled-」がダウンロードされます。
![binaryeditorexport.png](https://qiita-image-store.s3.amazonaws.com/0/201344/52e55a21-d4f7-365f-dece-959146b182eb.png)

ファイル名をhands-on.jpgなどに変更して、画像ビュアーで開いてみましょう。
画像は見れましたか？

# おわりに

今回は取得した画像をシリアルでPCに送りましたが、ESP32自体に保存して再利用することも考えられます。私はSPIFFSでファイル保存(SPIFFS.write)を試したのですが、何故か2018/02/18時点でバイナリ保存が出来ませんでした。書き込みサイズが0byteとなります。テキストでは保存できたのでBase64変換での保存など試してみたいです。

また、今回はESP32だけでなく、ESP8266との共通コードになるようにしていましたが、ESP8266ではどうしても接続時にException(28)が発生して期待通りに動きませんでした。そのうち解析してみたいです。
![esp8266exception28.png](https://qiita-image-store.s3.amazonaws.com/0/201344/a3bea24d-e93c-d955-d69d-ea5d492de707.png)

ESP8266でビルドする場合は、ボードの種類をESP32 Dev Module以外にします。
[Rasbee New バージョン NodeMcu Lua ESP8266](https://www.amazon.co.jp/dp/B01LW038VO/ref=pe_2107282_266464282_TE_3p_dp_1)と
[HiLetgo ESP8266 NodeMCU](https://www.amazon.co.jp/gp/product/B072K7P4JL/ref=pe_2107282_266464282_pd_te_s_s_ti/355-6229657-7770327?_encoding=UTF8&pd_rd_i=B072K7P4JL&pd_rd_r=87K2H3Q2HZ94KBX2D1AE&pd_rd_w=IpxNU&pd_rd_wg=QDhzq)を使用し、以下のボード設定で試しました。
![ESP8266arduinoIDE.png](https://qiita-image-store.s3.amazonaws.com/0/201344/5daa123e-ee23-d1ea-4161-39afd506ee04.png)

RICOH THETA Vの[ロードマップ](https://theta360.com/ja/about/theta/v/roadmap.html)に無線LANクライアントモードが記載されており、[2018/1/26](https://theta360.com/ja/info/news/2018-01-26/)に対応したようです。今回のコードに[アクセスポイント登録](https://developers.theta360.com/ja/docs/v2.1/api_reference/commands/camera._set_access_point.html)処理を入れておきました。次回はこちらを試したい♪