# 試験的な実装
第１章　設計の方針　で述べている考え方に基づいて、試験的な実装を試みている。
それらの実装のリポジトリのリストを示す。

- https://github.com/katsunori-waragai/disparity-view
 視差データをnpyファイルで保存してあるのを可視化するツール。
　おまけの機能として、ZED2iカメラからのステレオ画像の保存ツールを同梱。
- https://github.com/katsunori-waragai/libIGEVStereo
　ステレオ計測アルゴリズムの１例を、モジュールとしてインストール可能なように改変したもの。
　保存した結果は、上述のviewer によって可視化できる。

