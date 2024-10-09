# framework の選択
画像処理・機械学習の分野では、ベースとなる学習・評価の時点で、どのようなフレームワークを使うのかが重要になる。
最終成果物の推論の環境とは必ずしも一致しない。

## 学習・評価用の環境のframework

学習用・評価用の環境は、実装するアルゴリズムのソースコードの利用している環境になるだろう。
PyTorch, Tensorflow, Keras などが使われているだろうか。

[PyTorch](https://pytorch.org/)
Cudaデバイス以外でも利用できるようになってきている。Mac用の版も存在する。

pip install torch

[Tensorflow](https://www.tensorflow.org/?hl=ja)
NVIDIA GPU、Google TPUで動作する。

pip install tensorflow

[onnx](https://onnx.ai/)

- [Keras](https://keras.io/)
Kerasは、主にTensorflowをバックエンドとして動作します。
pip install keras

- [huggingface](https://huggingface.co/)
pip install huggingface
LLM系の実装が多い。
- [Ultralytics](https://github.com/ultralytics/ultralytics)
「detection, segmentation, classification, tracking and pose estimation」が用意されている。
pip install ultralytics

- [MMDetection](https://github.com/open-mmlab/mmdetection)
- MMDetectionのフレームワークでは以下のリンクに示すように多数のタスクにある実装が公開されている。
[Projects in OpenMMLab](https://github.com/open-mmlab/mmdetection3d?tab=readme-ov-file#projects-in-openmmlab)

[物体検出モデルがよりどりみどり　MMdetectionの使いかた](https://qiita.com/john-rocky/items/4d8888bee7a4592f1fc8)


#### 学習用・評価用のフレームワークに対する私見
- あなたが関心を持っているタスクの分野で一番に使われているフレームワークを使うべきだと考える。
- 学習用・評価用のデータセットの規模は年々大きくなるし、DVC（Data Version Control）や他のMLopsのフレームワークを使うことになるだろう。
- 開発チームの方針に合わせることだ。

#### 新規に考える必要性は少ないフレームワーク
Caffe
Chainer


## deploy先のframe work

deploy 先のframework　は、ボードやアクセラレータを選択した時点で決まるだろう。

複数のタスクを実装に加えたときの、処理能力・演算の負荷を考慮して、ボード・アクセラレータを選択する。
その結果を検証することになる。




