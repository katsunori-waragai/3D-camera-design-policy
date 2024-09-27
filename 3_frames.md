# frames
カメラからの立体形状の理解については、次の6通りの組合せがある。 
6通りに分割された各範囲の中では、カメラやアルゴリズムが置換え可能であることを期待する。
6通りについてそれぞれ、利点・欠点が異なる。


| |mono camera | stereo camera (3D camera) |
|---|----|---------------------------|
| each frame | (1) | (2)                       |
| multi frame | (3) | (4)                       |
| multi view | (5) | (6)                       |


## case1: each frame
- 利点：
    - 枠組みが簡単になる。単一フレームで処理ができるので、単一フレームで結果が出せる。他の処理と組合せやすい。
    - 単一フレームで評価できるので、評価が簡単になる。
    - 多様性のあるデータを処理する時に、独立なデータを増やしやすい。
- 欠点：
    - 時系列的に安定しない。精度が出にくい。 
- 単一フレームで可能な出力
  - 単一の視点位置からのdepth画像、もしくはそれを変換した点群データになる。
## case2: multi frame
- 利点：
    - 複数フレームの情報を使うので、時系列的に安定しやすい。
    - 対象物とカメラ間の相対的な移動を検出できる。
    - ベイス推定を用いることで、それまでに観測した範囲だけを使って、尤もらしい値を返すことができる。
- 欠点：
    - 複数フレームを使う分だけ処理が複雑になりやすい。 
- 最新フレームで可能な出力
  - 最新フレームの視点位置からのdepth画像、もしくはそれを変換した点群データになる。
## case3: multi view
- 独立性のある十分な数のデータがある。
- 利点：
    - 十分にまんべんなく収集されたデータを使うので、３Dの復元が容易になっていることが期待される。
    - 十分に数が集まってから復元に着手するので、無駄な作業が発生しない。
- 欠点：
    - 一度に大量のデータを処理するために、メモリの消費量・計算量が大規模になってしまう。
    - リアルタイム性を求めることはできない。
- 課題：
  - 動いてかつ変形している物体の扱いが難しい。
  - 何を実施し、何を実施しないのか選択が難しい。
- 最新フレームで可能な出力
  - 最新フレームの視点位置からのdepth画像、もしくはそれを変換した点群データになる。
  - case2の場合に比べて、物体の見えている情報が多いので、点群としての欠損が少ない。


### ６通りの場合分け

(1) 単眼深度（depth）推定
- paperswithcode [Monocular Depth Estimation](https://paperswithcode.com/task/monocular-depth-estimation)

　 [depth-anything](https://depth-anything.github.io/)
　 depth-anything-v2
　最近、伸びている分野。単眼カメラでdepthを算出できる。
　学習によって単眼カメラの画像からdepthを推定するもの。
　（ライブラリ自体は視差相当量を出すものになっている。そのため深度の絶対値は不明である）

(2) stereo libraries
  従来は、ステレオ計測の手法によって算出されるもの。近年は対応点がとれない領域に対しての深層学習（もしくはViT）によって推測されるものになっている。
(3) Dynamic-multiframe-depth

単一フレームでの単眼深度推定では、時系列としての安定性が欠けている場合が多いです。
そのため、移動方向や速度を算出するには、不安定すぎて使いものにならない。
人が対象物への絶対的な深度を推定できているとは、私には思えない。
できているのは、対象物間の深度の大小関係、深度の時系列的な安定性であると思う。
そのできている範囲の中でコーヒカップの位置と自分の手の位置とを評価し、
自分の手の位置を補正するように動かす。

- [Dynamic-multiframe-depth](https://github.com/ruili3/dynamic-multiframe-depth)
- [ProDepth: Boosting Self-Supervised Multi-Frame Monocular Depth with Probabilistic Fusion](https://github.com/Sungmin-Woo/ProDepth)
- [Generating Consistent Long Depth Sequences for Open-world Videos](https://depthcrafter.github.io/)
pdf [DepthCrafter: Generating Consistent Long Depth Sequences for Open-world Videos](https://depthcrafter.github.io/pdf/DepthCrafter.pdf)

https://x.com/NaveenManwani17/status/1831426496797536512


(4) 調査中
[Fast Multi-frame Stereo Scene Flow with Motion Segmentation](https://taniai.space/projects/cvpr17_fsf/)
pdf [Fast Multi-frame Stereo Scene Flow with Motion Segmentation](https://arxiv.org/pdf/1707.01307)

注意：multi-frame でのデータセットの例が、圧倒的に車載カメラに偏っている。
そのため、ロボットの頭部で見る画像がロボットの歩行や頭部の姿勢変化によって変わるような動画での例が
少なそうである。

(6)
- [Generalizable Novel-View Synthesis using a Stereo Camera](https://jinwonjoon.github.io/stereonerf/)

本論文では、特に多視点ステレオカメラ画像を対象とした、一般化可能なビュー合成アプローチを初めて提案する。近年のステレオマッチングが正確な形状予測を実証していることから、我々はステレオマッチングを新規ビュー合成に導入し、高品質な形状再構成を実現する。この目的のため、本論文では、ステレオマッチングをNeRFベースの一般化可能なビュー合成アプローチに統合する、StereoNeRFと呼ばれる新しいフレームワークを提案する。StereoNeRFは、ノービュー合成においてステレオマッチングを効果的に利用するための3つの主要コンポーネント、すなわち、ステレオ特徴抽出器、奥行きガイド付き平面掃引、ステレオ奥行き損失、を備える。さらに、StereoNVSデータセットを提案する。StereoNVSデータセットは、実シーンと合成シーンの両方を含む、初のステレオカメラ画像のマルチビューデータセットである。我々の実験結果は、StereoNeRFが一般化可能なビュー合成において、これまでのアプローチを凌駕していることを示している。

DeepL.com（無料版）で翻訳しました。

