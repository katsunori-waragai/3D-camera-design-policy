# 試験的な実装
第１章　設計の方針　で述べている考え方に基づいて、試験的な実装を試みている。
それらの実装のリポジトリのリストを示す。

### https://github.com/katsunori-waragai/disparity-view
 視差データをnpyファイルで保存してあるのを可視化するツール。
　おまけの機能として、ZED2iカメラからのステレオ画像の保存ツールを同梱。
### https://github.com/katsunori-waragai/libIGEVStereo
　ステレオ計測アルゴリズムの１例を、モジュールとしてインストール可能なように改変したもの。
　保存した結果は、上述のviewer によって可視化できる。

### https://github.com/katsunori-waragai/depth-anything-zed
 単眼画像に対する深度予測のライブラリにdepth-anythingがある。
 それをJetson AGX orin に移植してある実装を、モジュールとしてインストール可能なように改変したもの。
　それを利用したサンプルとして、ZED　SDKでの深度にスケーリングを合わせ込んだ単眼深度画像（正確には単眼での視差相当画像）を作成するスクリプトを含む。


### https://github.com/katsunori-waragai/jetson-llm-docker
　NVIDIA Jetson AGX orin で動作確認したLLMへのDocker設定
　それぞれのリポジトリは、Dockerfile の中で git clone している。
　そのためこのリポジトリの中では、オリジナルのリポジトリをfork していない。
##### egohos-docker/
	https://github.com/owenzlz/EgoHOS
##### grounded-sam-docker/
	https://github.com/IDEA-Research/Grounded-Segment-Anything
![https://github.com/IDEA-Research/Grounded-Segment-Anything/raw/main/assets/ram_grounded_sam_new.png]
##### groundingdino-docker/
	https://github.com/IDEA-Research/GroundingDINO
##### nanoowl-docker/
	https://github.com/NVIDIA-AI-IOT/nanoowl
	既にJetson対応済み
![](https://github.com/NVIDIA-AI-IOT/nanoowl/blob/main/assets/jetson_person_2x.gif)
##### nanosam-docker/
	https://github.com/NVIDIA-AI-IOT/nanosam
	既にJetson対応済み
![](https://github.com/NVIDIA-AI-IOT/nanosam/raw/main/assets/basic_usage_out.jpg)
