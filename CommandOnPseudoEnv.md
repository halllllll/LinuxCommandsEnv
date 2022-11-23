# What's this

できるだけローカルを汚さないように、いつも使うようなコマンド集を集めた`devcontainer`.

開発用のコマンドは入れない方針

# spec

Ubuntu22.04ベース([MSのdevcontainersのやつ](https://github.com/microsoft/vscode-dev-containers/tree/v0.238.0/containers/ubuntu))

- go version go1.18.3 linux/arm64
- python 3.10.5
- ruby ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [aarch64-linux]
- node v16.15.1
- npm 8.11.0

# tools
- wget
- scdl
- yt-dlp
- [ojichat](https://github.com/greymd/ojichat)
- ffmpeg
- spleeter
  - python製音声分離ソフト (参照: [Mac で Spleeter を使ってみた](https://qiita.com/S_Katz/items/d528d221927e6929ab8e))
  - ffmpegのあとに入れる
- whisper-cpp
  - ルート直下、`/whisper.cpp`にある。
  - モデルは`medium`を入れている。ほかにほしければ、以下のコマンドを叩いて確認してみよう。`.en`つきは英語専用（多言語非対応？）らしい
  -
  ```
  $ bash /whisper.cpp/models/download-ggml-model.sh 
  Usage: /whisper.cpp/models/download-ggml-model.sh <model>

  Available models: tiny.en tiny base.en base small.en small medium.en medium large
  ```  
  - 16khz wavしか扱えないらしい。ffmpegによる変換の例は以下
  - 
  ```
  ffmpeg -i ./drive/fisherwiki2_cpp.wav -ar 16000 ./drive/fisherwiki2_cpp_16khz.wav
  ```
  - useage
  - 明示的にオプションで指定しないと、テキストファイルが吐き出されない。吐き出される場所は`-f`と同ディレクトリだった
  ```
  $ /whisper.cpp/main  --language ja --model /whisper.cpp/models/ggml-medium.bin -f ./drive/fisherwiki2_cpp_16khz.wav -v -pc -ovtt -otxt
  whisper_model_load: loading model from '/whisper.cpp/models/ggml-medium.bin'
  ```
