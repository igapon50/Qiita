<!--
title:   動画の音声から文字起こしする
tags:    Python,SpeechRecognition,動画,文字起こし
id:      2aae838aab22feb2eb6a
private: false
-->
# 目的
動画内で話している内容を文字列に変換してファイルに保存する。

# 背景
前回のNo.3記事により、shotcutのmltプロジェクトファイルに動画を追加出来た。次はテロップを追加するための文字列を、動画の音声から文字起こしして用意する。

- No.3記事「[shotcutのタイムラインに動画を追加する](2021-12-04_MLT_Python_XML_shotcut_af5470b75eff1f140332.md)」
- No.2記事「[Pythonとffmpegで動画の無音部分をカットする](2021-05-05_Python_ffmpeg_3faa83fc8af1543bc672.md)」
- No.1記事「[shotcutで動画作ってyoutube投稿してみた](2021-04-24_YouTube_shotcut_5486f187d7751af2ddd5.md)」

# 環境
- Windows10 バージョン21H1(OSビルド19043.1348)
 - Python3.9.1
 - SpeechRecognition 3.8.1
- 動画撮影：iPhoneX

# セットアップ
これまでの記事のセットアップを実施して、追加で以下のライブラリをセットアップする。
- Windowsキーを押す
- 検索ボックスに「cmd」を入力して決定する
- 以下のコマンドを実行する

```commandline:cmd.exe
pip install SpeechRecognition
```

# 使い方
[Git](https://github.com/igapon50/training/releases/tag/1.4.0)からSource code(zip)をダウンロードして、展開する。
展開したパスを「c:\Git\traning\」として、加工前の動画ファイルが「c:\temp\test.mov」なら以下のコマンドを実行する。

```commandline:cmd.exe
cd c:\Git\traning\python\Movie
python movieHelper.py c:\temp\test.mov
```
動画と同じフォルダに、動画と同じファイル名で、文字起こしのテキストファイルが作成される。

自分のコードにimportして使うなら、movieHelper.pyファイルを取り込み、以下の通り記載する。

```commandline:python
from movieHelper import MovieHelper
mh = MovieHelper('c:/temp/test.mov')
mh.mov_to_text()
```

# おわりに
私は調べていませんが、SpeechRecognitionは最大で1分程度の音声ファイルしか変換できないそうです。
なるべく長く分割して変換しないと、文章が分かれてしまうので、良い変換がされないように思います。
そのうち、最適な区切り方も考えてみたいです。
次回は今回用意した文字列をテロップにする予定です。

# 関連リンク
## 参考サイト
以下の記事を参考にさせていただきました。ありがとうございました。

- hirasu1231@ハムレット型エンジニアさん「[pythonで長い音声・動画からの文字起こしを実装する](https://www.hamlet-engineer.com/posts/mojiokoshi_long.html)」
- yukara_13さん「[PythonでWAVファイルを読み込む](https://yukara-13.hatenablog.com/entry/2013/11/09/103848)」
- siddhantsomaniさん「[.wavファイルを複数の.wavファイルに分割する方法は？](https://www.webdevqa.jp.net/ja/python/wav%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E8%A4%87%E6%95%B0%E3%81%AEwav%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AB%E5%88%86%E5%89%B2%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%E3%81%AF%EF%BC%9F/825724990/)」

## ドキュメント

- [Git SpeechRecognition](https://github.com/Uberi/speech_recognition#readme)
- [FFmpeg](https://trac.ffmpeg.org/wiki/CompilationGuide)