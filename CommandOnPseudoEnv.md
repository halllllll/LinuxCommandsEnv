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
