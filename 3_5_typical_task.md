# 代表的なタスク
- 物体検出
- セグメンテーション
- インスタンスセグメンテーション
- 物体検出
- 属性分類
- 姿勢推定
  - PapersWithCode [Pose Estimation](https://paperswithcode.com/task/pose-estimation)
- 顔照合・人物推定(re-identification)
  - 顔照合は、マスク顔対応が終わった時点で、技術分野としては枯れた技術になりつつあります。（黒人対応はありますが）
  - 利用予定のプラットフォームにある実装をまず調査してください。
  - 対応OS,対応言語、依存ライブラリのバージョン、GPUなどのアクセラレータ対応の状況、今後のサポートの状況などを調査してください。
  - 利用予定のカメラと想定しているユースケースでの顔画像を使って、評価します。
- open vocabulary での物体検出
PapersWithCode [Open Vocabulary Object Detection](https://paperswithcode.com/task/open-vocabulary-object-detection)

[Open vocabulary object detection with NVIDIA Grounding-DINO](https://www.nvidia.com/ja-jp/on-demand/session/other2024-tao55gdino/)

- open vocabulary でのセグメンテーション
- PapersWithCode [Open Vocabulary Semantic Segmentation](https://paperswithcode.com/task/open-vocabulary-semantic-segmentation)


## 自律移動ロボットでよくあるタスク
- SLAM
- 自己位置推定
- 経路計画

### 最近出てきた・増えてきた画像認識タスク
##### 3D Semantic Segmentation
PapersWithCode [3D Semantic Segmentation](https://paperswithcode.com/task/3d-semantic-segmentation)


[3D Object Detection From Stereo Images](https://paperswithcode.com/task/3d-object-detection-from-stereo-images)

##### affordance の検出
##### 物体の検出にともなうハンドによる把持動作の生成に近い分野

- PapersWithCode [Grasp Generation](https://paperswithcode.com/task/grasp-generation)
- PapersWithCode [hand-object pose](https://paperswithcode.com/task/hand-object-pose)
- PapersWithCode [3D Canonical Hand Pose Estimation](https://paperswithcode.com/task/3d-canonical-hand-pose-estimation)


##### 多視点カメラ画像による物体の理解
  - 従来は、カメラパラメータが違う多数のweb画像で特定の建築物を復元するようなものが多かった。
    - 組み込み可能性は考えない。
  - 今は、一つのカメラから視点位置を変えて、特定物を見て、形状復元するのが増えてきている。
    - 組み込み可能性を気にしだしている。

PapersWithCode [3D Reconstruction](https://paperswithcode.com/task/3d-reconstruction)

##### Visual Question Answering (VQA)
- PapersWithCode [Visual Question Answering (VQA)](https://paperswithcode.com/task/visual-question-answering)

#### VLA(vision language action)
視覚と言語の入力があって、動作を生成するタスクとして定義される。
例：視覚を含む各種センサデータ、運転についての指示がある状況で、自律運転車の動作を生成するタスク
このなかでは、人検出などの物体検出・周辺環境のセグメンテーションもVLAのend-to-end の中に隠れたタスクとなる。



## 大規模言語モデルは、避けて通れない
#### PC・クラウドサービスでの推論
主に国内の動向から

[Heron A Library for Vision / Video and Language models](https://github.com/turingmotors/heron)

#### 組み込みデバイスでの推論

[Openな資産とFPGAを活用して、領域特化型のProcessorを作ろう！](https://www.docswell.com/s/TIER_IV/ZG6LJK-2022-04-26-162717)

github [Swan A Lightweight Language Model Execution Environment Using FPGA](https://github.com/turingmotors/swan)
[言語モデルを高位合成でFPGAに実装してみた](https://zenn.dev/turing_motors/articles/82505880d27d65)

