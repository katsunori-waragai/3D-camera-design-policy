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
- 画像の入力・出力を切り替えるのはおおごとである。
  - 例：「１６台の監視カメラ入力のそれぞれに対して検出・追跡処理をして、描画結果を４ｘ４に配置してモニタに表示させる」
  - このようなアプリケーションをC++やPythonで１つのアプリケーションとして作るのは得策ではない。
  - H.264のビデオ信号のdecode などを必要とするだろうし、4x4の１６台が5x6の３０台に置き換わるかもしれない。
  - それらを１つの言語で記述してリンクして動作させることが簡単ではなくなってくる。

## GStreamerもしくはNVIDIA DeepStreamで期待されること
- 上記の１から４の処理がパイプラインとして別々のプログラムして動作する。
- それぞれが別のプロセスであるので、マルチコアの性能を引き出しやすい。
- 処理が、エレメントという単位に分割される。
- エレメントをパイプ "!" で連結することができるので、巨大な処理を１つに記述しなくて済む。
- かなりのエレメントは、既存のエレメントを流用すればよい。
- 自作する必要のあるエレメントは、既に入力と出力が単純化されているので、自作部分を単純化できる。
- 

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
- [DeepStream Python Apps](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps)
- 

