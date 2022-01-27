<!--
title:   shotcutのタイムラインに動画を追加する
tags:    MLT,Python,XML,shotcut
id:      af5470b75eff1f140332
private: false
-->
# 目的
前回記事[「Pythonとffmpegで動画の無音部分をカットする」](2021-05-05_Python_ffmpeg_3faa83fc8af1543bc672.md)により、たくさん動画を作ったので、shotcutのタイムラインに追加する処理を作ってみた。

# 背景
前々回記事[「shotcutで動画作ってyoutube投稿してみた」](2021-04-24_YouTube_shotcut_5486f187d7751af2ddd5.md)、前回記事に引き続き、shotcutのmltファイルを直接加工して、動画を追加するpythonコードを作ってみた。

# 環境

- Windows10 バージョン21H1(OSビルド19043.1348)
 - Python3.9.1
 - Shotcut 21.10.31

# セットアップ
以下の記事のセットアップを参照のこと。
前回記事[「Pythonとffmpegで動画の無音部分をカットする」](2021-05-05_Python_ffmpeg_3faa83fc8af1543bc672.md)
前々回記事[「shotcutで動画作ってyoutube投稿してみた」](2021-04-24_YouTube_shotcut_5486f187d7751af2ddd5.md)

# 使い方
[GitHub](https://github.com/igapon50/training/releases/tag/1.3.0)からSource code(zip)をダウンロードして、展開する。
展開したパスを「c:\Git\traning\」として、加工前の動画ファイルが「c:\Git\traning\python\Movie\test.mov」なら以下のコマンドを実行する。

```
cd c:\Git\traning\python\Movie
python movieCutter.py c:\Git\traning\python\Movie\test.mov
```
すると、「\*_part\*.mov」な分割ファイルが生成されるので、以下のようなスクリプトを書いて実行すると、プレイリストに分割ファイルが追加され、test1.mltに保存される。

```python:テストコード：プレイリストに動画追加
from shotcutHelper import *
# カレントフォルダ以下で「*_part*.mov」を再起検索して、
# 見つけたファイルをトラックmail_binに追加して、保存する
app = ShotcutHelper('./せんちゃんネル/テンプレート.mlt')
movies = glob.glob('./**/*_part*.mov', recursive=True)
app.add_movies('main_bin', movies)
app.save_xml('./せんちゃんネル/test1.mlt')
```
（プレイリストのIDは'main_bin'である）
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/646d7888-3664-4ccf-f440-f040d5706070.png)
↓　追加された
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/08383521-4fa2-ebc3-f7fd-d5ca397d3e5e.png)


また、以下のようなスクリプトを書いて実行すると、タイムラインに分割ファイルが追加され、test2.mltに保存される。

```python:テストコード：タイムラインに動画追加
from shotcutHelper import *
# カレントフォルダ以下で「*_part*.mov」を再起検索して、
# 見つけたファイルをトラックplaylist0に追加して、保存する
app = ShotcutHelper('./せんちゃんネル/テンプレート.mlt')
movies = glob.glob('./**/*_part*.mov', recursive=True)
app.add_movies('playlist0', movies)
app.save_xml('./せんちゃんネル/test2.mlt')
```
（ただし、存在する動画トラックのIDが'playlist0'とする）
shotcutのタイムラインのV1のIDは、
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/7b56a153-559d-f1e4-723a-715c64a29c8d.png)
mltファイルを直接開くと、以下の赤線の様にID="playlist0"であることが確認できる
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/bba283e5-94f7-56c7-cd46-9f8f5894431c.png)
↓　タイムラインに追加された（二つ目のCloudsは、隙間がないように前詰めされたようだ）
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/4fa76c89-0aa3-18a1-796a-cbb27c1d550e.png)

# おわりに
やっていることは、ただのxml編集なので、難しいところはなかった。
トラックの追加とテロップの追加もそのうち挑戦してみたい。

# 関連リンク
## 参考サイト
[Pythonの順序付き辞書OrderedDictの使い方](https://note.nkmk.me/python-collections-ordereddict/)
[Python 日付フォーマット チートシート](https://python.civic-apps.com/date-format/)
## ドキュメント
[MLT XML Annotations](https://shotcut.org/notes/mltxml-annotations/)