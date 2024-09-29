# design policy
- アルゴリズムを置換え可能にする。
  - 入力出力が同じ形式のアルゴリズムは、置換え可能なようにインタフェースを設計する。
- 目的が同じ３Dカメラのインタフェースも、可能な範囲で置換え可能にする。
  - ３Dカメラを用いた上位のアプリケーションと、3Dカメラ自体の機能とはインタフェースで切り離した設計にする。
  - これによって、３Dカメラを用いた上位のアプリケーションを１から作り直すことを減らす。
- 表示系の機能を、個々のアルゴリズム・個々の3Dカメラの実装から切り離す。
  - 表示系の開発コストを削減する。
  - 画像ファイルでの確認用のインタフェース
  - リアルタイム表示用のインタフェース
  - 3D表示用のインタフェース 
- アルゴリズム利用の際の結果データのデータ形式
  - ネットワーク間でそのデータを転送する際のインタフェース


## class ベースの設計
設計の例
- インスタンスを生成する際に、最小限の設定をすれば、動作すること
- defaultでも動作できるようにしておく
- メソッドの実行の際には、画像を渡せば実行できるようにする。

#### 後処理込みのAPI
推奨：機械学習を含む場合でも、ユーザーに公開するAPIにおいては、後処理込みのものとすること
非推奨： 深層学習後のネットワークの出力そのままを返すので、解釈可能にするためにユーザー側が後処理を別に実施する。


## 画像データの選択肢

- １：np.ndarray型 (numpy, opencv)
  - np.ndarray型の場合には、色順序がRGBなのかBGRなのかは明示する。
  - cv2.imread(filename)
  - skimage.io.imread(filename)
  - imageio.imread(filename)
  np.ndarray型では、画像の画素値を整数値で扱う場合もあるし、float型を使う場合もある。
  float型の使われ方としては、[0.0 - 1.0]の間の連続値として用いられている場合もある。
  skimageの方がfloat型の場合の対応されている範囲が広い。
- ２：PIL.Image型
  - Image.open(imfile)
- 3: open3d.geometry.Image型
  - open3d.io.read_image(filename)
- 4: Torch, TorchVision
Tensor 型では、データをCPUだけではなく、GPUを指定できる。
（そのため、処理をGPUで完結させたいときには、CPUとGPU間のデータ転送を発生させないようにコーディングするのがのぞましい。）
- torch.from_numpy()

- class torchvision.tv_tensors.Image(data: Any, *, dtype: Optional[dtype] = None, device: Optional[Union[device, str, int]] = None, requires_grad: Optional[bool] = None
https://pytorch.org/vision/main/generated/torchvision.tv_tensors.Image.html
- 5: tf.Tensor 型 Tensorflow
 GPU, TPUに対応している。

#### 商用ライブラリの独自データ形式
- 商用の画像認識ライブラリは、独自のデータ型を用いている場合がある。
- そして多くの場合は、np.ndarray型と相互変換できる補助関数を用意している。

#### サードパーティ依存の画像データの例
- 1: StereoLabs ZED SDK の　sl.Mat() 型
- 画像の取得時刻情報などを含んだデータ形式であり、get_data()でnp.ndarray型に変換できる。
- ただ、チャネル数が４になっていることに注意

```
import pyzed.sl as sl
zed = sl.Camera()
image = sl.Mat()
zed.retrieve_image(image, sl.VIEW.LEFT, sl.MEM.CPU, display_resolution)
cv_img = image.get_data()
```

#### 画像認識・機械学習系にデータを渡す場合の注意
- チャネル数が、そのAPIで使用するものと一致していること
- 部分画像を渡したいときには `image[100:200, 300:400, :].copy()` などとして連続な領域として渡す

#### GPU上にその画像データはおけるのか
TensorFlowのTensor型ではデータは、CPU、GPUのどちらにも置ける。
しかし、大半の画像データ形式は、CPU上に限られる。


## test
ロボットに使われる画像計測・画像認識についてトレース可能なテストがほしい。
そのカメラとそのアルゴリズムに対して、どのようなデータでどのようなテストをしているのかが
トレースできる環境を構築する。

#### testの方針
- 個々のリポジトリごとにテストを行う。
- アルゴリズム自体のテストのためには、カメラが不要でテストする。
- テスト用に使用するデータは、誰でもがダウンロード可能にして、誰でもがテスト可能な状況を目指す。
- CircleCIなどでは対応していないボード・CPUの場合には、ローカルなデバイス上でテスト自動化を目指す。
###### センサなしでのテスト
画像計測アルゴリズム・画像認識アルゴリズムの単体テストは、センサなしでも実現できる。
入力データをファイルにしておいて、それを読んで動作させて、結果を期待値と比較することでテストが可能である。
画像データは、pngファイルなどの損失のないデータを用いる。
pythonの場合：npyファイル, npz ファイルを用いることで、numpyで表されるデータを保存できる。
それらを使うことで、センサなしで、アルゴリズムへの単体テストができる。

###### センサ特化での再現性試験
- センサとそのSDKによっては、センサデータを独自形式でファイルに保存できるものがある。
　それを使うと、カメラデバイスを外した状況で、あたかもカメラを接続したような動作ができる。
　そのことによって、同じセンサデータの時系列を与えたときの出力を、アルゴリズムの種類・パラメータの変更によってどうなるのかを比較できる。
　例： StereoLabs ZED SDK
  [recording/playback](https://github.com/stereolabs/zed-sdk/tree/master/recording/playback)

######  ROS2での再現試験
ros2bag という仕組みがあって、センサデータをほrecordしplayback する機能がある。
- [Recording and playing back data](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html)
- [Recording and playing back data](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html)

## implementation
#### python3の場合
```commandline
python3 -m pip install .
```
でモジュールをパッケージとしてインストールできるようにする。
あるいは、pypi に登録して
```commandline
python3 -m pip install パッケージ名
```
ができることを目指す。
そのため、モジュール名、パッケージ名は既存のpypi に登録してあるのと名前がぶつからないように付ける。

###### headless への対応
組み込み分野の画像計測・画像認識処理では、本番の運用の中では、画像表示を持たないことが多い。
そのため、アルゴリズムの実行の中では、画面表示をもつべきではない。
Linuxの場合にはX11の表示先を期待してはならない。
cv2.imshow()を必要としてはならない。

###### 標準出力
- アルゴリズムのモジュール自体は標準出力にモジュールはメッセージを表示すべきではない。
- 実装先のシステムが用意しているlogシステムに出力する。
- pythonの場合にはloggingがある。

## C++(C++11以降)の場合
なるべく最新のC++の規格を採用したコーディングを心がけましょう。
- [The OpenCV Coding Style Guide](https://github.com/opencv/opencv/wiki/Coding_Style_Guide)
- [Open3D style guide](https://www.open3d.org/docs/0.12.0/contribute/styleguide.html)
- [ROS2 coding style](https://docs.ros.org/en/rolling/The-ROS2-Project/Contributing/Code-Style-Language-Versions.html#id1)
 ROS2では`Rolling targets C++17.` と言っている。
 3Dカメラを利用したアルゴリズムもそれに従うのがよい。


CMakeを使ってライブラリの作成、サンプルの実行形式を作れるようにしましょう。

## data file format
#### データ形式は、極力標準的なデータ形式を採用する。
- CPUの種類(bit数、エンディアン）によらないデータ形式を用いる。
- 機械可読性が必要とされるテキストファイルの場合には、以下のようなコンピュータ言語によらず可読な標準的なデータ形式を用いる。
  - json
  - yaml
  - toml
- pythonの場合の数値データの保存
  - npy ファイルを用います。NaN, PosInf, NegInfなどの値を持つことができます。
- OpenEXR: ハイダイナミックレンジイメージのための画像ファイルフォーマット
- ply: 点群データのファイルフォーマット。Open3Dで読み書きできます。
#### 使わないデータ形式
pickle
 pythonのオブジェクトをほとんどなんでも保存できてしまうデータ形式です。
　そのため、loadしたpickle オブジェクトを含むなにかを実行した時に、何が起こるかわからないという致命的な欠陥があります。
