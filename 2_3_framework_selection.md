# framework の選択
画像処理・機械学習の分野では、ベースとなる学習・評価の時点で、どのようなフレームワークを使うのかが重要になる。
最終成果物の推論の環境とは必ずしも一致しない。

## 学習・評価用の環境のframework

学習用・評価用の環境は、実装するアルゴリズムのソースコードの利用している環境になるだろう。
PyTorch, Tensorflow, Keras などが使われているだろうか。

PyTorch

Tensorflow

Keras

huggingface

Ultralytics
github https://github.com/ultralytics/ultralytics

MMDetection

[物体検出モデルがよりどりみどり　MMdetectionの使いかた](https://qiita.com/john-rocky/items/4d8888bee7a4592f1fc8)



## deploy先のframe work

deploy 先のframework　は、ボードやアクセラレータを選択した時点で決まるだろう。

複数のタスクを実装に加えたときの、処理能力・演算の負荷を考慮して、ボード・アクセラレータを選択する。
その結果を検証することになる。




