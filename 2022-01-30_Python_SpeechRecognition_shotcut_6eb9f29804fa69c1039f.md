<!--
title:   shotcutのタイムラインに字幕を追加する
tags:    Python,SpeechRecognition,shotcut,動画,文字起こし
id:      6eb9f29804fa69c1039f
private: false
-->
# 目的
shotcutのmltプロジェクトファイルに、動画内で話している内容を字幕として追加する。

# 背景
前回のNo.4記事により、動画の音声から文字起こしすることが出来た。次はshotcutのmltプロジェクトファイルに字幕として追加する。

- No.4記事「[動画の音声から文字起こしする](2021-12-14_Python_SpeechRecognition_2aae838aab22feb2eb6a.md)」
- No.3記事「[shotcutのタイムラインに動画を追加する](2021-12-04_MLT_Python_XML_shotcut_af5470b75eff1f140332.md)」
- No.2記事「[Pythonとffmpegで動画の無音部分をカットする](2021-05-05_Python_ffmpeg_3faa83fc8af1543bc672.md)」
- No.1記事「[shotcutで動画作ってyoutube投稿してみた](2021-04-24_YouTube_shotcut_5486f187d7751af2ddd5.md)」

# 環境
- Windows10 バージョン21H1(OSビルド19043.1466)
 - Python3.9.1
 - SpeechRecognition 3.8.1
- 動画撮影：iPhoneX

# セットアップ
これまでの記事のセットアップを実施する。

# 使い方
[Github](https://github.com/igapon50/training/releases/tag/1.6.0)からSource code(zip)をダウンロードして、展開する。
展開したパスを「c:\Git\traning\」として、shotcutのmltプロジェクトファイルが「c:\temp\test.mlt」、加工前の動画ファイルが「c:\temp\test.mov」なら以下のコマンドを実行する。

```commandline:cmd.exe
cd c:\Git\traning\python\Movie
python addMov2mlt.py c:\temp\test.mlt c:\temp\test.mov
```
shotcutのmltプロジェクトファイルと同じフォルダに、動画と字幕が追加されたmltプロジェクトファイル「test_addMov2mlt.mlt」が作成される。
※動画と同じフォルダに、中間ファイルが生成される。

自分のコードにimportして使うなら、movieHelper.pyとmltHelper.pyファイルを取り込み、以下の通り記載する。

```commandline:python
import os
from movieHelper import MovieHelper
from mltHelper import MltHelper
mlt_file_path = 'c:/temp/test.mlt'
target_file_path = 'c:/temp/test.mov'
target_folder = os.path.dirname(mlt_file_path)
target_mlt_basename = os.path.basename(mlt_file_path)
target_mlt_file_name = os.path.splitext(target_mlt_basename)[0]
target_mlt_file_ext = os.path.splitext(target_mlt_basename)[1]
create_mlt_path = os.path.join(target_folder, target_mlt_file_name + '_addMov2mlt' + target_mlt_file_ext)
# mltヘルパー生成
app = MltHelper(mlt_file_path)
# トラックを二つ追加
playlist_id_V2 = app.add_track('V2')
playlist_id_V3 = app.add_track('V3')
# 動画ヘルパー生成
mh = MovieHelper(target_file_path)
# 無音部分をカットした動画に分割する
movie_list = mh.movie_dividing()
# 動画をプレイリストに追加
app.add_movies('main_bin', movie_list)
# 動画をタイムラインに追加
app.add_movies(playlist_id_V2, movie_list)
# 字幕をプレイリストに追加
app.add_subtitles('main_bin', movie_list)
# 字幕をタイムラインに追加
app.add_subtitles(playlist_id_V3, movie_list)
# mltの保存
app.save_xml(create_mlt_path)
```

# 説明
mltHelperは、新規作成したmltファイルに対応していないので、あらかじめshotcutで、トラックを一つ追加して保存する必要がある。
例えば、以下の様なテンプレートmltファイルをあらかじめ用意しておいて、そこに追加するとよい。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/18d71374-3a61-0ce9-2382-147ad4c23b72.png)
以下の様に、分割後の動画がV2トラックに、字幕がV3トラックに追加される。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/e206adef-cdf3-326c-636d-2a4a0d891e07.png)
また、原因はわかっていないが、文字起こしに失敗するケースがある。その場合はexeptionのメッセージが字幕に設定される。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/1bf94e05-0c8c-0430-9d0a-d1c9846b0071.png)
字幕の文章は、自分の耳で確認して修正すること。
動画や字幕は、まとめて選択して、開始位置を調整するとよいと思う。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/201344/96754e03-94f0-6f97-44ed-7a557a54bc96.png)

加工前：[こちらの動画](https://youtu.be/oQP_KmSWO1k)の27秒目辺りから2分10秒目辺り。
加工後：[こちらの動画](https://youtu.be/cMpRP683QZA)の9秒目辺りから41秒目辺り。
※元の動画の音声が小さいため、ゲイン・音量+20dbしてyoutubeに上げている。

無音部分をカットした動画分割処理のパラメータを変更したい場合は、以下の引数を設定する。

```
def movie_dividing(self,
                   threshold=0.05,
                   min_silence_duration=0.5,
                   padding_time=0.1,
                   ):
    """
    動画ファイルから無音部分をカットした部分動画ファイル群を作成する
    作成した動画のファイルパスリストを返す
    一時的にWaveファイルを作るが、使い終わったら削除する

    :param threshold: float 閾値
    :param min_silence_duration: float [秒]以上thresholdを下回っている個所を抽出する
    :param padding_time: float [秒]カットしない無音部分の長さ
    :return: list[str] 分割したファイルのパスリスト
    """
```

# おわりに
動画の音量が小さかったためか、いい感じに拾えてませんが、字幕を入れるのは楽になりそうです。
音量を上げるために、(何故かまとめて設定できずに)一つずつ動画にフィルター追加でゲイン・音量+20dbする必要があった。
次は、指定のトラックの動画すべてにまとめてフィルター設定する処理を作ってみたい。

# 関連リンク
## 参考サイト
以下の記事を参考にさせていただきました。ありがとうございました。

- nkmkさん「[Pythonの順序付き辞書OrderedDictの使い方](https://note.nkmk.me/python-collections-ordereddict/)」

## ドキュメント

- [Python Software Foundation](https://www.python.org/)