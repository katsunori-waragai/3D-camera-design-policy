# open3d
特定のSDKに依存しない形で、３Dのデータを扱うためのライブラリとしてはOpen3Dがある。
### 利用性の高い機能
- ３Dデータを扱うためのデータ形式
- 視差データを深度データに変換する。
- 深度データを点群データにする。
- 点群データの座標変換を実施する
- 視点を設定して点群データを２次元画像に射影する。

### 組込時には使わない、開発時に利用する機能
- 結果の表示機能
  - 結果の表示機能は、結果をネットワーク越しに受け渡して、作業用PCで表示させるという使い方もある。


[Open3D](https://www.open3d.org/)

Open3Dは、3Dデータを扱うソフトウェアの迅速な開発をサポートするオープンソースのライブラリです。
Open3Dのフロントエンドは、C++とPythonの両方で、厳選されたデータ構造とアルゴリズムのセットを公開している。
バックエンドは高度に最適化され、並列化のために設定されている。
Open3Dは、クリーンな状態から開発され、依存関係は小さく、慎重に考慮されています。
最小限の労力で、さまざまなプラットフォームでセットアップし、ソースからコンパイルすることができます。
コードはクリーンで、一貫したスタイルになっており、明確なコードレビューのメカニズムによってメンテナンスされています。
Open3Dは、多くの研究プロジェクトで使用されており、クラウドにも積極的に導入されています。
オープンソースコミュニティからの貢献を歓迎します。

プロジェクトページから和訳

[Open3D-ML](https://github.com/isl-org/Open3D-ML)
An extension of Open3D to address 3D Machine Learning tasks

## Open3DとカメラのSDKとの連携
RealSense とOpen3Dとの状況
https://github.com/IntelRealSense/librealsense/blob/master/wrappers/open3d/readme.md

## CUDAが利用可能な環境では、Open3DもCUDA対応の版を使うのがよい。

[Open3D with CUDA](https://zenn.dev/yutashx/scraps/577ae6230ebb82)

## OpenGL
Open3DはOpenGLを内部で利用している。
OpenGLの記述能力は細かい部分まで可能であるが、ライブラリを使いこなすのがむずかしい。
Open3Dから表示させた方が、表示が楽だろう。

## Point Cloud Library
https://pointclouds.org/

https://github.com/strawlab/python-pcl
This repository has been archived by the owner on Dec 31, 2023. It is now read-only.


https://pypi.org/project/pclpy/
リリース日: 2018年8月25日
なので、python binding　は最近は更新されていない。


[SSII2018のTSを例題に，PCL (C++)とOpen3D (Python) の比較](https://qiita.com/n_chiba_/items/fc9605cde5c19a8c7dad)
```commandline
まとめ
まだPCLほど高機能ではないが，Pythonで点群処理ができるのは素晴らしい．実行速度をあまり気にしないときはOpen3Dを使うのが良さげ．
各点ごとに面倒な処理をするとか，PCLにしか実装されていないとか，実行速度が重要なときとかはPCL．
```

# ３D カメラのSDKでの点群情報のデータ構造
## ZED SDK
[ZED SDKを使いこなす　点群のデータの３通り](https://qiita.com/nonbiri15/items/64ff6efa4b22d3bfb43a)


[ZED SDK Getting Point Cloud Data #](https://www.stereolabs.com/docs/depth-sensing/using-depth#getting-point-cloud-data)


