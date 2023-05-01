# scdl
公式から引用
```
# Download track & repost of the user QUANTA
scdl -l https://soundcloud.com/quanta-uk -a

# Download likes of the user Blastoyz
scdl -l https://soundcloud.com/kobiblastoyz -f

# Download one track
scdl -l https://soundcloud.com/jumpstreetpsy/low-extender

# Download one playlist
scdl -l https://soundcloud.com/pandadub/sets/the-lost-ship

# Download only new tracks from a playlist
scdl -l https://soundcloud.com/pandadub/sets/the-lost-ship --download-archive archive.txt -c

# Sync playlist
scdl -l https://soundcloud.com/pandadub/sets/the-lost-ship --sync archive.txt

# Download your likes (with authentification token)
scdl me -f
```

# ffmepg

## 4k high framerate

[YouTubeオススメのffmpegのエンコードオプション設定例](https://qiita.com/hotstaff/items/c64e27bb0c0317084515#%E3%82%B3%E3%83%94%E3%83%9A%E3%81%A7%E4%BD%BF%E3%81%88%E3%82%8B%E3%82%B3%E3%83%BC%E3%83%89)からのパクリ

`ffmpeg -i input.mp4 -movflags +faststart -c:v libx264 -profile:v high -level:v 4.0 -b_strategy 2 -bf 2 -flags cgop -coder ac -pix_fmt yuv420p -crf 23 -maxrate 85M -bufsize 170M -c:a aac -ac 2 -ar 48000 -b:a 384k output.mp4`

## extract audio/video/subtitle(/data?) from video

たとえばwavだけほしけりゃ`-vn`オプションをつければいい。`video none`か？

[ffmpeg Documentation #4 Stream selection](https://ffmpeg.org/ffmpeg.html#Stream-selection)によると

> ffmpeg provides the -map option for manual control of stream selection in each output file. Users can skip -map and let ffmpeg perform automatic stream selection as described below. The -vn / -an / -sn / -dn options can be used to skip inclusion of video, audio, subtitle and data streams respectively, whether manually mapped or automatically selected, except for those streams which are outputs of complex filtergraphs. 

ということで、映像だけ・音声だけ・字幕だけを抜き取るオプションがそれぞれある。`-dn`のデータストリームってのがよくわからない

## mp4 to gif
高画質で動画ファイルをgifアニメに。

`ffmpeg -i input.mov -filter_complex "[0:v] fps=10,scale=640:-1,split [a][b];[a] palettegen [p];[b][p] paletteuse" output-palette.gif`

# yt-dlp

## エラーが出た時
アップデートすれば治ることがある（あった）
```
sudo yt-dlp -U
```

## [basic] DL先指定
アウトプット先のパスを指定する`-P`オプション。以下は、適当な動画を、カレントディレクトリの`drive`フォルダの`youtube`フォルダを指定する例
```
yt-dlp "URL" -P drive/youtube
```

## [basic] コーデック&&フォーマット指定

（コマンドのサンプルの参照先URLはあるけど、アフィ臭さがあるので、ここでは貼り付けない）

コーデックは`-S`、フォーマットは`-f`オプションでそれぞれ指定する。試してないが、オプションの引数は`""`で囲ってもいい気がする。次なるは、最高画質かつ最高音質で`h264`でエンコーディングするらしい？

```
yt-dlp "URL" -f bestvideo[ext=mp4]+bestaudio[ext=m4a] -S vcodec:h264
```
あと、以上のように、`bestvideo`と`bestaudio`を`+`でくっつけることができるっぽい

## max high qualty SOUND ONLY dl
[yt-dlpの使い方-簡易版#最高品質で音声のみダウンロードする](https://zenn.dev/almon/articles/f5952bf9047608#%E6%9C%80%E9%AB%98%E5%93%81%E8%B3%AA%E3%81%A7%E9%9F%B3%E5%A3%B0%E3%81%AE%E3%81%BF%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%89%E3%81%99%E3%82%8B)

以下は`m4a`の例。別のフォーマットがよければ`--audio-format`をmp3とか好きなやつにする
```
yt-dlp -x -f "ba[ext=webm]" --audio-format m4a "[URL]"
```
`ba`はたぶん`bastaudio`のショートハンド。`-x`は`--extrat-audio`のショートハンド

# whisper-cpp

モデルをダウンロードするコマンド。`.en`つきは英語専用（多言語非対応？）らしい

  ```
  $ bash /whisper.cpp/models/download-ggml-model.sh 
  Usage: /whisper.cpp/models/download-ggml-model.sh <model>

  Available models: tiny.en tiny base.en base small.en small medium.en medium large
  ```  
  **16khz wav**しか扱えないらしい。ffmpegによる変換の例は以下
  
  ```
  ffmpeg -i ./drive/fisherwiki2_cpp.wav -ar 16000 ./drive/fisherwiki2_cpp_16khz.wav
  ```
  
  明示的にオプションで指定しないと、テキストファイルが吐き出されない。吐き出される場所は`-f`と同ディレクトリだった
  
  ```
  $ /whisper.cpp/main  --language ja --model /whisper.cpp/models/ggml-medium.bin -f ./drive/fisherwiki2_cpp_16khz.wav -v -pc -ovtt -otxt -osrt
  whisper_model_load: loading model from '/whisper.cpp/models/ggml-medium.bin'
  ```
