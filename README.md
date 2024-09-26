# 3D-camera-design-policy
3D camera design policy document

## Purpose
- 3D カメラを用いた以下の処理を組合せを簡単にし、アルゴリズムの差し替えを簡単にするインタフェースがほしい。
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

## カメラ・OSへの依存性の少ないライブラリ
- Open3D

## 表示方法の問題

## 検出結果の受け渡しの問題
