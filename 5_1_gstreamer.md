# GStreamer もしくはNVIDIA DeepStream

動画像関係の処理をする際に、GStreamer もしくはNVIDIA DeepStream　という枠組みを使うという選択肢もある。

## よくある処理(GStreamerを使わない）
1. カメラから画像を読む。
2. 画像を縮小する。
3. 物体検出をする。
4. 検出結果を表示する。
これらが1つのプログラムの中で呼び出されます。
#### 課題
- その言語、そのライブラリの範囲でのマルチスレッド化の対応。
- そのプログラムの中で、それぞれのデータを管理しなくちゃならない。
- プログラムが巨大化しがち。
- メンテナンスが難しくなりやすい。

## GStreamerもしくはNVIDIA DeepStreamで期待されること
上記の１から４の処理がパイプラインとして別々のプログラムして動作する。
それぞれが別のプロセスであるので、マルチコアの性能を引き出しやすい。
#### Gstreamer
- [GStreamerとは](https://www.argocorp.com/UVC_camera/GStreamer.html)
- [gstreamer備忘録](https://qiita.com/maueki/items/b54cbe5207bb16869756)
`$ gst-launch-1.0 videotestsrc ! autovideosink`
のような使い方をする。
- [RustでGStreamer Pluginを書く](https://qiita.com/uzuna/items/6c183253736e26598c45)

"tcambin ! videoconvert ! ximagesink"
このように "!" でパイプとして、それぞれのエレメントをつないで処理をおこなうことができる。

#### NVidia DeepStream
- [DeepStream SDKとは？](https://www.macnica.co.jp/business/semiconductor/articles/nvidia/134117/)
- [NVIDIA DeepStream SDK for Rust](https://github.com/aosoft/nvidia-deepstream-rs)
- 

