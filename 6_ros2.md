# ROS2
3D-camera-design-policy should be consistent with ROS2.

## ROS2を視野に入れるべき理由
- 十分に定式化されたタスクと、それに対する実装がROS2に存在する。
  - SLAM
  - 自己位置推定
- エンドユーザーに使ってもらうことに力を入れている３DカメラメーカーはROS2対応を行なっている。

## 脱ROS
- ROSの使い方としては、初期の実装の時点においてROSを使い、あとの実装ではROS以外の通信プロトコールを用いる例がある。
