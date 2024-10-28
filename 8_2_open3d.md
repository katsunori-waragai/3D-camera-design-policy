# open3d
特定のSDKに依存しない形で、３Dのデータを扱うためのライブラリとしてはOpen3Dがある。
### 利用性の高い機能
- ３Dデータを扱うためのデータ形式
- 視差データを深度データに変換する。
- example
- 深度データを点群データにする。
- 点群データの座標変換を実施する
- 視点を設定して点群データを２次元画像に射影する。
- https://github.com/isl-org/Open3D/blob/main/examples/python/geometry/point_cloud_to_rgbd.py

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

### 解釈
- バックエンドが最適化している
  - グラフィックス
  - 行列演算
  なので、自力でOpenGLなどを書く状況ではない。

[Open3D-ML](https://github.com/isl-org/Open3D-ML)
An extension of Open3D to address 3D Machine Learning tasks

## Open3DとカメラのSDKとの連携
RealSense とOpen3Dとの状況
https://github.com/IntelRealSense/librealsense/blob/master/wrappers/open3d/readme.md

## CUDAが利用可能な環境では、Open3DもCUDA対応の版を使うのがよい。

[Open3D with CUDA](https://zenn.dev/yutashx/scraps/577ae6230ebb82)


## Open3Dをのぞましいと私が考えている理由
- 画像データ・点群データ処理を行うバックエンド側のライブラリを、ユーザーはいじる必要がない。
- 画像データ・点群データの表示に関わる部分のバックエンド側のライブラリを、ユーザーはいじる必要がない。

### カメラの内部パラメータ open3d.camera.PinholeCameraIntrinsic
[open3d.camera.PinholeCameraIntrinsic](https://www.open3d.org/docs/latest/python_api/open3d.camera.PinholeCameraIntrinsic.html)
o3d.camera.PinholeCameraIntrinsic(width=width, height=height, fx=fx, fy=fy, cx=cx, cy=cy)

OpenCVで用いるカメラ行列
camera_matrix = np.array([[fx, 0, cx], [0, fy, cy], [0, 0, 1]])

[open3d.geometry.Image](https://www.open3d.org/html/python_api/open3d.geometry.Image.html)



## OpenCVとOpen3Dとの比較
- Open3Dでは、それぞれの変数がそれぞれの型を持っている。そのため、Open3Dを使っている範囲では、不適切な型の変数を引数に与えるとエラーになる。
- type hintを見た時点で、変数の意味が明確になりやすい。
  - OpenCVなど、ほとんどの変数がnp.ndarray型、もしくはcv::Mat型になってしまう。
- Open3Dの関数を用いて実装すると、だれが書いても似たようなコードになりやすい。
- Open3Dと名乗るだけあって、3Dのデータを扱うためのライブラリが充実している。OpenCVでは、ライブラリに多くの機能が不足しているので、視差を深度に換算するのさえ、自作のコードを必要としてしまう。

## OpenGL
Open3DはOpenGLを内部で利用している。
OpenGLの記述能力は細かい部分まで可能であるが、ライブラリを使いこなすのがむずかしい。
Open3Dから表示させた方が、表示が楽だろう。
### 付記： OpenGLでの記述例
- ZED SDK(StereoLabs)のサンプルコードは、3DのグラフィックにOpenGLを用いている。
- 画層認識・画像計測・それらの利用（ロボットの開発など）をしている人が、こういったコードを書くのに時間をさくのは難しいだろう。

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


## RealSense
[RealSenseで点群を取得する方法](https://qiita.com/kakuteki/items/73d4fce886a99230a2d6)

