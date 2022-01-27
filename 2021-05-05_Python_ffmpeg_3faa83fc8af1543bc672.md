<!--
title:   Pythonとffmpegで動画の無音部分をカットする
tags:    Python,ffmpeg
id:      3faa83fc8af1543bc672
private: false
-->
# 目的
Python + ffmpeg で動画ファイル(mov)から無音部分をカットして、残った部分をそれぞれ動画ファイルにする。

前回の記事「[shotcutで動画作ってyoutube投稿してみた](2021-04-24_YouTube_shotcut_5486f187d7751af2ddd5.md) 」


# 背景
shotcutで動画を編集する前に、素材動画から不要な無音部分をカットしたかった。

参考サイト「[PythonでYouTube用動画のカットとテロップ挿入を自動化してみた！](https://kajimublog.com/python-video-cut/)」のコードは、生成する動画ファイルが一つにまとまる様だが、分割されたファイルの方が、shotcutで使いやすいと思ったので、一部修正する。


# 環境

- Windows10 バージョン2004(OSビルド19041.928)
 - Python3.9.1
 - chocolatey 0.10.15
 - 動画編集ソフト：Shotcut Version UNSTABLE-21.02.09
 - 動画再生ソフト：MPC-HC(64-bit) バージョン1.7.10(d911f14)
- 動画撮影：iPhoneX 14.4.2
 - 動画撮影アプリ：Microsoft Pix


# セットアップ

- Windowsキーを押す
- 検索ボックスに「cmd」と入力
- Ctrl + Shift + Enterを押す(管理者として実行)
- 以下のコマンドを実行する

```commandline:cmd.exe（管理者として実行）
choco install ffmpeg
```

- 途中選択肢が出る場合は、すべてy + Enterする。
- 以下のコマンドを実行する

```commandline:cmd.exe（管理者として実行）
pip install soundfile
pip install numpy
pip install moviepy
```


# やっていること
大部分は参考サイト「[PythonでYouTube用動画のカットとテロップ挿入を自動化してみた！](https://kajimublog.com/python-video-cut/)」と「[動画の無音部分を自動でカットする](https://nantekottai.com/2020/06/14/video-cut-silence/)」のコードを流用した。ただし、コード一式が見つからず、結合してもそのままでは動かなかったので修正した。

- 動画ファイルから音声ファイルを生成する。
- 音声ファイルから無音部分を除いた、残す部分を特定する
- 残す部分の動画を個別に※生成する

※shotcutで動画を結合するのは簡単なので、無音部分をカットした後の動画ファイルをバラバラに生成するコードに修正した。


# 使い方
[Git](https://github.com/igapon50/training/releases/tag/1.2.0)からSource code(zip)をダウンロードして、展開する。
展開したパスを「c:\Git\traning\」として、加工前の動画ファイルが「c:\test.mov」なら以下のコマンドを実行する。

```
cd c:\Git\traning\python\Movie
python movieCutter.py c:\test.mov
```

加工前の動画ファイル名に「_part{:2d}」を付けた、以下のファイルが作成される。

```
c:\test_part01.mov
c:\test_part02.mov
・・・
```


# おわりに
ffmpegがとても素晴らしい。今後も是非活用したい。
参考サイトは、Git URLを見つけることができなかったので、すぐに試すことができなかった。とりあえずコード一式になり、試すことができたので良かった。


# 関連リンク
## 参考サイト

- [PythonでYouTube用動画のカットとテロップ挿入を自動化してみた！](https://kajimublog.com/python-video-cut/)
- [動画の無音部分を自動でカットする](https://nantekottai.com/2020/06/14/video-cut-silence/)
- [それFFmpegで出来るよ！](https://qiita.com/cha84rakanal/items/e84fe4eb6fbe2ae13fd8)


## ドキュメント

- [FFmpeg](https://trac.ffmpeg.org/wiki/CompilationGuide)