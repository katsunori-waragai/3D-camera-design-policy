# 開発

## Dockerを利用した個別モジュールの開発をベースとする。
host環境は、OSのバージョンの違い・CPUの違いが人それぞれに異なったものになります。
Dockerを用いることで、docker環境を構築することで、host環境の影響を減らすことができます。
Dockerfile を記載して docker build, docker runを実行すると、
各人の環境による違いを減らすことができる。

### Dockerfileの書き方
- 既存のプロジェクトでのDockerfile の書き方を参考にしてください。
- ベースとなるdocker imageを選びます。
  - 例：NVIDIA Jetson用のDocker image
  - 例：NVIDIA GPU を搭載したPC用のDocker image
  - 例：raspberry pi 用のDocker image
- Dockerfile に処理を書き加えていく
  - `RUN apt install -y パッケージ` など
  - pythonモジュールの依存ライブラリのインストールはDockerfileには書かず、pyproject.toml　に書きます。

### pyproject.toml の書き方(pythonの場合)
- 該当のリポジトリをモジュールをパッケージとしてインストールする手順を記述します。
- 従来 requirements.txt で記載していた内容は、pyproject.tomlに記載することになります。
- `python3 -m pip install .` でそのモジュールのインストールができます。
  - setup.py もsetup.cfg ファイルも今は不要になりました。


#### 理由：　画像認識・機械学習関係のライブラリは簡単にコンフリクトを生じやすい。
- 依存ライブラリのバージョンが相容れない。
- 同時複数のライブラリが同じデバイスを利用しようとして、コンフリクトを生じてしまう。
- 個々の実装の作者は、GPUを同時に別々のライブラリが使うことを想定していない。
  - 例：pytorchのGPU利用

#### 理由：　画像認識・機械学習関係のライブラリを追加インストールすると、既存の環境を壊しやすい
上記に同じ。


## CircleCI: config.yaml の書き方
- CircleCIを使ってテストの自動化は、UbuntuPCの場合には比較的簡単にできます。
  - 身近にいるソフトウェアエンジニアの詳しい人に教わってください。
  - CircleCIを使えば、Githubにコミットしたたびに、テストを自動化することができます。
## Github Actions
Nvidia JetsonやRaspberry Pi　などのようにクラウドサービスでは用意されない環境では、別な方法で単体テストの自動化を実現することになります。
- [JetsonでGitHub ActionsのSelf-hosted Runnerをサービスとして動かす](https://qiita.com/ynott/items/8ec1dbfe14f3225bbe70)


## TensorRTモデルへの変換
pytorchのモデルはTensorRTへのモデルに変換することで、NVIDIA　GPUでの動作速度を改善することができます。

## onnx　モデルへの変換
tensorflowやpytorchのモデルをonnx にモデルを変換することができます。
onnxは、インテルの各種デバイスで動作させることができます。

## C++ 実装へのpython バインディング
同じアルゴリズムをC++実装の他にpythonからも使いたいことがあります。
その場合にはC++のライブラリに対してpython バインディングを作る。
- pybind11

[次世代Pythonバインディングライブラリnanobindを試す](https://zenn.dev/neka_nat/articles/c01449087a7ff6)


----
## other board
画像認識・機械学習の移植先は、多数あります。
その開発環境は、それぞれに異なります。
それぞれのボードごとに調べてください。

#### 画像認識の推論用のデバイスというエッジな世界の経験
- Cuda デバイス
- Jetson Deep Learning Accelerator (DLA)
- Google TPU
- Intel Movidius
- OpenCV AI Kit
- HAILO
- Kakip
- 画像認識のFPGA実装

