# ROS2
3D-camera-design-policy should be consistent with ROS2.

## ROS2を視野に入れるべき理由
- 十分に定式化されたタスクと、それに対する実装がROS2に存在する。
  - SLAM
  - 自己位置推定
- エンドユーザーに使ってもらうことに力を入れている３DカメラメーカーはROS2対応を行なっている。

## ROS2 support
- https://github.com/IntelRealSense/realsense-ros
- https://github.com/orbbec/OrbbecSDK_ROS2
- https://github.com/stereolabs/zed-ros2-wrapper
- 

## dual OS システム中の部品としてのROS2
- ロボットの設計において、Linuxでリアルタイム性を確保するのは困難である。
- 一枚のボードコンピュータが、全てのセンサ・全てのアクチュエータをハンドリングするのは実際的ではない。
#### ありうる設計
- 動物において脊髄反射を行なっているような階層を、ある範囲のセンサとアクチェータの組合せについて導入する。
  - 時定数が短い部分の処理にファームウェアを導入する。
- 1枚のボードコンピュータ（マルチコア）の中のOSをdual OSシステムとする。
  - リアルタイムOSとLinuxのシステムとして、ROS2はLinuxに配置する。
## 脱ROS
- ROSの使い方としては、初期の実装の時点においてROSを使い、あとの実装ではROS以外の通信プロトコールを用いる例がある。

## 駆動対象がある場合
- エラー処理が特に重要になる。
  - どのようなエラー・例外が発生しても、安全に終了することが必要になる。
  - やってはいかないこと：すべての例外を捕捉して、処理をすること。
