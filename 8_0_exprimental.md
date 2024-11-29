# 試験的な実装
第１章　設計の方針　で述べている考え方に基づいて、試験的な実装を試みている。
それらの実装のリポジトリのリストを示す。

### https://github.com/katsunori-waragai/disparity-view
 視差データをnpyファイルで保存してあるのを可視化するツール。
 - 視差データをcolormapで表示
 - colormapの視差データと左画像との重ね書き
 - 法線ベクトルの向きによる色分け
 - 点群データからの再投影
 - 点群データのplyファイルへの保存
　おまけの機能として、ZED2iカメラからのステレオ画像の保存ツールを同梱。
### https://github.com/katsunori-waragai/libIGEVStereo
　ステレオ計測アルゴリズムの１例を、モジュールとしてインストール可能なように改変したもの。
  IGEVは近年のステレオライブラリの１つ。
　細い領域に対しても視差の計算ができるようになってきている。
　保存した結果は、上述のviewer によって可視化できる。

### https://github.com/katsunori-waragai/libstereosgbm
 ステレオ計測アルゴリズムの１例、ここではOpenCVのStereoSGBMを、同様なインタフェースをもつモジュールとしてインストール可能なように改変したもの。
　実装自体に新規性はない。

### https://github.com/katsunori-waragai/depth-anything-zed
 単眼画像に対する深度予測のライブラリにdepth-anythingがある。
 それをJetson AGX orin に移植してある実装を、モジュールとしてインストール可能なように改変したもの。
　それを利用したサンプルとして、ZED　SDKでの深度にスケーリングを合わせ込んだ単眼深度画像（正確には単眼での視差相当画像）を作成するスクリプトを含む。

### https://github.com/katsunori-waragai/yolox-zed-sdk
YOLOX とZED　SDKとの組合せた実装例

### https://github.com/katsunori-waragai/zed-gsam
grounded-segment-anything with ZED SDK

![](https://github.com/katsunori-waragai/zed-gsam/raw/main/figures/depth_and_gsam.png)

Open Vocaburary でのセグメンテーションとZED SDKでの連動の実装例

- 深度情報とセグメンテーションとを画素単位で対応付けすることができる。
- ZED SDK の算出する世界座標系と、Open Vocaburary でのセグメンテーションとを対応付けることができる。
- できないこと：
  - text_prompt　で指定されていない物体へのセグメンテーション。
  - 物体の見えてない範囲についての仮説の生成
  - 次の行動を引き起こすための、把持位置についての仮説の生成
  - 周辺環境を考慮して、ぶつからないためのアームとハンドの行動計画の生成
  - これらの課題については、それぞれに適した実装を探すこと。


-----------------------------------------------

# 動作確認済み・ライブラリ化未実施の実装
- 大規模言語モデルがJetsonで動作することの検証結果
  - Dockerfileでの動作
  - オリジナルは git clone をすることにした。


### https://github.com/katsunori-waragai/jetson-llm-docker
　NVIDIA Jetson AGX orin で動作確認したLLMへのDocker設定
　それぞれのリポジトリは、Dockerfile の中で git clone している。
　そのためこのリポジトリの中では、オリジナルのリポジトリをfork していない。
##### egohos-docker/
-　https://github.com/owenzlz/EgoHOS
- 自己視点画像での腕検出と対象物の検出 対象物は1st orderと 2nd orderとがある。
![](https://github.com/owenzlz/EgoHOS/blob/main/demo/twohands_obj1_optimized.gif?raw=true)
![](https://github.com/owenzlz/EgoHOS/blob/main/demo/twohands_obj2_optimized.gif?raw=true)
![](https://github.com/owenzlz/EgoHOS/blob/main/demo/twohands_optimized.gif?raw=true)
##### grounded-sam-docker/
- https://github.com/IDEA-Research/Grounded-Segment-Anything0
- Segmentation AnythingをOpen Vocabulary と関連付けたもの。
- Open Vocabularyで、物体の領域がピクセル単位でわかる。
![https://github.com/IDEA-Research/Grounded-Segment-Anything/raw/main/assets/ram_grounded_sam_new.png]
##### groundingdino-docker/
- https://github.com/IDEA-Research/GroundingDINO
- 自然言語で指定した記述にそって物体検出ができる。
- 文章で指定したものをなんでも検出できるAI Grounding DINO
##### nanoowl-docker/
- https://github.com/NVIDIA-AI-IOT/nanoowl
- Jetson対応済み
- Jetsonで高速に操作するように移植されたOWL-ViT 
- これも自然言語で指定した記述にそって物体検出ができる。
![](https://github.com/NVIDIA-AI-IOT/nanoowl/blob/main/assets/jetson_person_2x.gif)
##### nanosam-docker/
- https://github.com/NVIDIA-AI-IOT/nanosam
- 既にJetson対応済み
- Jetsonで高速に操作するように移植されたSegmantation Anything
![](https://github.com/NVIDIA-AI-IOT/nanosam/raw/main/assets/basic_usage_out.jpg)
