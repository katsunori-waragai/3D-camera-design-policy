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

## requirement
- 全ての時刻情報はCPUボード、各種センサボードで同期されていること
- 各種センサからの取得データには、時刻情報が付随していること。
- 画像センシングの結果には、必ず元となった画像データの取得時刻の情報を含んでいる
- データのスナップショットをとる操作と、そのデータを転送する操作とは区別されるべきである

## あればなおうれしい項目
- 複数センサのデータのスナップショットを同期してとれれば、うれしい。
- 複数センサのデータの統合処理が楽になる。

## test
ロボットに使われる画像計測・画像認識についてトレース可能なテストがほしい。
そのカメラとそのアルゴリズムに対して、どのようなデータでどのようなテストをしているのかが
トレースできる環境を構築する。

#### testの方針
- 個々のリポジトリごとにテストを行う。
- アルゴリズム自体のテストのためには、カメラが不要でテストする。
- テスト用に使用するデータは、誰でもがダウンロード可能にして、誰でもがテスト可能な状況を目指す。
- CircleCIなどでは対応していないボード・CPUの場合には、ローカルなデバイス上でテスト自動化を目指す。

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

モジュール名、パッケージ名は既存のpypi に登録してあるのと名前がぶつからないように付ける。

## C++(C++11以降)の場合
なるべく最新のC++の規格を採用したコーディングを心がけましょう。
- [The OpenCV Coding Style Guide](https://github.com/opencv/opencv/wiki/Coding_Style_Guide)
- [Open3D style guide](https://www.open3d.org/docs/0.12.0/contribute/styleguide.html)

