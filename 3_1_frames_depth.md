# depth

深度の推定は、３Dの再構築としてとらえ直すことができる。



| |mono camera | stereo camera (3D camera) |
|---|----|---------------------------|
| each frame | (1) | (2)                       |
| multi frame | (3) | (4)                       |
| multi view | (5) | (6)                       |

### ６通りの場合分け

(1) 単眼深度（depth）推定
- paperswithcode [Monocular Depth Estimation](https://paperswithcode.com/task/monocular-depth-estimation)

　 [depth-anything](https://depth-anything.github.io/)
　 depth-anything-v2
　最近、伸びている分野。単眼カメラでdepthを算出できる。
　学習によって単眼カメラの画像からdepthを推定するもの。
　（ライブラリ自体は視差相当量を出すものになっている。そのため深度の絶対値は不明である）

[Combining zed2 and segment anything](https://community.stereolabs.com/t/combining-zed2-and-segment-anything/2989)


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
- ![](https://github.com/ruili3/dynamic-multiframe-depth/raw/main/pictures/dynamic_depth_result.gif)
- [ProDepth: Boosting Self-Supervised Multi-Frame Monocular Depth with Probabilistic Fusion](https://github.com/Sungmin-Woo/ProDepth)
- [Generating Consistent Long Depth Sequences for Open-world Videos](https://depthcrafter.github.io/)
pdf [DepthCrafter: Generating Consistent Long Depth Sequences for Open-world Videos](https://depthcrafter.github.io/pdf/DepthCrafter.pdf)

https://x.com/NaveenManwani17/status/1831426496797536512


[14. Multi-Frame Stereo](http://stereo--vision.com/kiji14e.html)
[Fast Multi-frame Stereo Scene Flow with Motion Segmentation](https://arxiv.org/abs/1707.01307)


(4) 調査中
[Fast Multi-frame Stereo Scene Flow with Motion Segmentation](https://taniai.space/projects/cvpr17_fsf/)
pdf [Fast Multi-frame Stereo Scene Flow with Motion Segmentation](https://arxiv.org/pdf/1707.01307)

注意：multi-frame でのデータセットの例が、圧倒的に車載カメラに偏っている。
そのため、ロボットの頭部で見る画像がロボットの歩行や頭部の姿勢変化によって変わるような動画での例が
少なそうである。

[Generalizable Novel-View Synthesis using a Stereo Camera](https://jinwonjoon.github.io/stereonerf/)
本論文では、特に多視点ステレオカメラ画像を対象とした、一般化可能なビュー合成アプローチを初めて提案する。
近年のステレオマッチングが正確な形状予測を実証していることから、我々はステレオマッチングを新規ビュー合成に導入し、
高品質な形状再構成を実現する。
この目的のため、本論文では、ステレオマッチングをNeRFベースの一般化可能なビュー合成アプローチに統合する、
StereoNeRFと呼ばれる新しいフレームワークを提案する。
StereoNeRFは、ノービュー合成においてステレオマッチングを効果的に利用するための3つの主要コンポーネント、
すなわち、ステレオ特徴抽出器、奥行きガイド付き平面掃引、ステレオ奥行き損失、を備える。
さらに、StereoNVSデータセットを提案する。
StereoNVSデータセットは、実シーンと合成シーンの両方を含む、初のステレオカメラ画像のマルチビューデータセットである。
我々の実験結果は、StereoNeRFが一般化可能なビュー合成において、これまでのアプローチを凌駕していることを示している。

DeepL.com（無料版）で翻訳しました。
[pdf](https://jinwonjoon.github.io/stereonerf/StereoNeRF_files/main.pdf)
[StereoNeRF](https://github.com/Haechan21/StereoNeRF)
StereoNVS-Real: CC BY-NC 3.0 DEED (Non-commercial use)
BlendedMVS-stereo: CC BY-NC 3.0 DEED (Non-commercial use)


[Neural Fields for Structured Lighting](https://openaccess.thecvf.com/content/ICCV2023/papers/Shandilya_Neural_Fields_for_Structured_Lighting_ICCV_2023_paper.pdf)


[stereo_slam](https://github.com/srv/stereo_slam)


(6)
- [Generalizable Novel-View Synthesis using a Stereo Camera](https://jinwonjoon.github.io/stereonerf/)

本論文では、特に多視点ステレオカメラ画像を対象とした、一般化可能なビュー合成アプローチを初めて提案する。近年のステレオマッチングが正確な形状予測を実証していることから、我々はステレオマッチングを新規ビュー合成に導入し、高品質な形状再構成を実現する。この目的のため、本論文では、ステレオマッチングをNeRFベースの一般化可能なビュー合成アプローチに統合する、StereoNeRFと呼ばれる新しいフレームワークを提案する。StereoNeRFは、ノービュー合成においてステレオマッチングを効果的に利用するための3つの主要コンポーネント、すなわち、ステレオ特徴抽出器、奥行きガイド付き平面掃引、ステレオ奥行き損失、を備える。さらに、StereoNVSデータセットを提案する。StereoNVSデータセットは、実シーンと合成シーンの両方を含む、初のステレオカメラ画像のマルチビューデータセットである。我々の実験結果は、StereoNeRFが一般化可能なビュー合成において、これまでのアプローチを凌駕していることを示している。

DeepL.com（無料版）で翻訳しました。

[Depth Estimation from Monocular/Stereo Images](https://github.com/PINTO0309/PINTO_model_zoo/tree/main?tab=readme-ov-file#7-depth-estimation-from-monocularstereo-images)
このサイトには、深度推定に関わる学習済みモデルが用意されている。


---
- Github [Awesome-Deep-Stereo-Matching](https://github.com/fabiotosi92/Awesome-Deep-Stereo-Matching?tab=readme-ov-file#frameworks)
 A curated list of awesome Deep Stereo Matching resources
- pdf [A Survey on Deep Stereo Matching in the Twenties](https://arxiv.org/pdf/2407.07816v1)