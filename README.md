# 3D-camera-design-policy
3D camera design policy document

## Purpose
- 3D カメラを用いた以下の処理を組合せを簡単にし、アルゴリズムの差し替えを簡単にするインタフェースがほしい。
この文書は、３Dカメラのユーザーが、カメラ画像を取得する・深度を取得する・検出を実施するなどの処理を行う際に
再利用が容易になるモジュールの設計を簡単にする指針を示すことを目標とする。

## 3D cameras
- カメラの種類
  - Stereo Camera
  - ToF Camera
  - Structured light Camera
  - monocular camera　(exceptional)
- 機能
  - 画像取得
  - 視差の計算
  - カメラのIMU、timestamp の取得
  - 点群への換算
  - 物体検出
    - 物体検出結果の３Dでの座標取得
  - 物体検出のセグメンテーション
    - セグメンテーションベースの物体検出結果の３Dでの座標取得
  - open vocabulary　での物体検出
  - open vocabulary　でのセグメンテーション
  - 物体の2Dでの追跡
  - 物体の3Dでの追跡
  - 特定の対象物でのポーズ推定
    - (２D, 3D)
    - 人の全身・頭部・手
    - 車両のポーズ推定
    - その他
    - 

## カメラ・OSへの依存性の少ないライブラリを推奨する。
- 推奨例：
- Open3D

## 表示方法の問題
- アルゴリズムのモジュールにおいては、画像表示・点群表示・top viewの表示自体もライブラリ化した部分には含めない。
- 理由：画像認識モジュールは、head less のシステムで利用されることが想定されるため。

## 検出結果の受け渡しの問題
- モジュールを実装する言語によらない受け渡しの方法があること
  - 例：protobuf
- 同一言語内では、その言語内で便利な受け渡し方法があってよい。
  - 例：dataclasses-json, pyserde

# 目次
## 第１章　設計の方針
[design policy](1_0_design_policy)
## 第２章　３Dカメラの選択
[camera_selection](2_1_camera_selection)
[board_selection](2_2_board_selection.md)
[framework_selection](2_3_framework_selection.md)
## 第３章　フレームの扱いと代表的なタスク
- [frames](3_0_frames)
- [frames_depth](3_1_frames_depth.md)
- [代表的なタスク](3_5_typical_task.md)
## 第４章　遅延時間とカメラ姿勢の問題
[reducing latency and camera pose](4_reducing_latency.md)
## 第５章　スレッド処理
[スレッド処理](5_threads.md)
## 第６章 ROS2
[ROS2](6_ros2.md)
## 第７章 3Dカメラでの計測と物体検出についての提案の私見
[usage_proposal](7_1_usage_proposal)
[3D計測と画像認識の未来](7_5_future.md)
## 第８章　試験的な実装
- [exprimental](8_0_exprimental)
- 開発 [development](8_1_development.md)
- Open3D [open3d](8_2_open3d.md)
- [task design](8_3_task_design.md)
- [移植](8_4_porting.md)
- [ドキュメント](8_5_module_doc.md)
- [アルゴリズムの精度](8_6_alogrithm_accuracy.md)
## 第９章 対象外のこと
[out_of_scope](9_out_of_scope.md)

## あなたにできること
- このプロジェクトのドキュメントを自由に展開してください。
- このプロジェクトで紹介するOSSの実装を自由にご利用ください。
- 関連する有益な情報の入手先のURLを教えてください。反映します。
