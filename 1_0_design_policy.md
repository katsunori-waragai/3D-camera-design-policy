# この構想の目的
- 3Dカメラの利用と、画像認識・３D再構築・機械学習などを扱いやすくする設計方針を示すこと
- アルゴリズムの置き換えを簡単にしていくこと
- ３Dカメラそれ自体についてのインタフェースについて、SDKごとの差異をわかりやすくする。
- 可能ならば、それらが類似のインタフェースによってデータの取得できること。

## 参考事例
scikit-learn

scikit-learnでは、学習アルゴリズムが異なってもfit(), predict()
という共通の名前をメソッドに使っていることで、
アルゴリズムを差し替えを簡単にしている。

```.py:
from sklearn import svm
X = [[0, 0], [1, 1]]
y = [0, 1]
clf = svm.SVC()
clf.fit(X, y)
clf.predict([[2., 2.]])
```


RandomForestClassifierの場合

```.py:
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import make_classification
X, y = make_classification(n_samples=1000, n_features=4,
                           n_informative=2, n_redundant=0,
                           random_state=0, shuffle=False)
clf = RandomForestClassifier(max_depth=2, random_state=0)
clf.fit(X, y)
clf.predict(X)
```

どちらの実装の利用においても
```commandline
clf.fit(X, y)
clf.predict(X)
```

部分は共通である。
このため、分類器　Classifierの実装が置換えやすいインタフェースになっている。

混同行列(confusion marix)を計算して、どういう誤分類を生じているのかをテーブルにまとめるのは
共通の方法で対処できる。

[sklearn.metrics.confusion_matrix(y_true, y_pred, *, labels=None, sample_weight=None, normalize=None)]
(https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html#sklearn.metrics.confusion_matrix)

```.py:
>>> from sklearn.metrics import confusion_matrix
>>> y_true = [2, 0, 2, 2, 0, 1]
>>> y_pred = [0, 0, 2, 2, 0, 2]
>>> confusion_matrix(y_true, y_pred)
array([[2, 0, 0],
       [0, 0, 1],
       [1, 0, 2]])
```

このような共通のインタフェース設計が、機械学習のライブラリを利用しやすくした。

共通のインタフェースの利点は、以下のようなコードで、4種類のアルゴリズムに対して、
fit(), predict()が共通なことが効いている。

```.py:
# Fit estimators
ESTIMATORS = {
    "Extra trees": ExtraTreesRegressor(
        n_estimators=10, max_features=32, random_state=0
    ),
    "K-nn": KNeighborsRegressor(),
    "Linear regression": LinearRegression(),
    "Ridge": RidgeCV(),
}

y_test_predict = dict()
for name, estimator in ESTIMATORS.items():
    estimator.fit(X_train, y_train)
    y_test_predict[name] = estimator.predict(X_test)
```

[引用元のスクリプト](https://scikit-learn.org/stable/auto_examples/miscellaneous/plot_multioutput_face_completion.html#sphx-glr-auto-examples-miscellaneous-plot-multioutput-face-completion-py)

![](https://scikit-learn.org/stable/_images/sphx_glr_plot_multioutput_face_completion_001.png)
スクリプトの実行結果
このデモスクリプトは、顔の上半分を与えたら、顔の下半分を予測させるモデルの学習後の予測の結果だ。
顔の特徴は、上半分と下半分で無関係なわけがない。顔が丸顔だったら、上半分と下半分で共通だろし、年齢や性別の影響も上半分と下半分で共通に現れると期待される。
だから、上半分の顔画像を与えたときに、下半分の顔を予想させようという取り組みが可能になる。
使用した手法によっては、それらしい結果を与えている。
（生成AIを用いた状況では、これらよりも良い推論を得られるだろう。）

このようにアルゴリズムが異なっても同一のインタフェースを持つことの利点は大きい。

## この宣言がめざすこと
- タスクへの入出力が同じアルゴリズムであれば、アルゴリズムの種類が変わっても、同一のインタフェース設計でコードを書ける。
- 例：単一フレームでのステレオ計測のアルゴリズムの実装であれば、利用するアルゴリズムの種類や実装のフレームワークが違っても、 同じインタフェースで利用可能な状況を目指すこと。
- 依存するCPUボード・アクセラレータが変わっても、有用なアルゴリズムが移植可能な状況を実現しやすくすること。
  - 3Dカメラを差し替えても、可能な範囲で画像認識・３D計測が同一のインタフェースで組合せ可能にしていくこと
    - 例：物体検出の結果を３Dカメラからの３Dデータと関連付けるを簡単にしていくこと。
- 3Dカメラの種類の違いによる比較をしやすくしていくこと
  - 現状では、それぞれのSDKでの表示ツールでの表示に頼っていることが多い。
  - そのため、違いが表示ツールによるものなのか、特性の差によるものかがわかりにくい
  - 共通の表示ツールを使えるようになれば、特性の差を比較しやすくなる。
- ある3DカメラのSDKのサンプルプログラムが有用でしかもOSSのライセンスであることがある。
  - そのよいアイディアを、別のカメラでも使いたい。

#### 3Dカメラと画像認識との統合は、新しい産業の基盤となる
- 機械学習が、従来はファイルに変換済みのデータに対するもので、身体性をあまり持たないものが中心だった。
- しかし、大規模言語モデルの成功は、それらをカメラとアーム・ハンドをもつ身体性のある知能に変えつつある。
- 視覚言語モデルに基づく汎用性が高い学習は、アーム・ハンドと結びついて、新たな時代を切り開こうとしている。
- 視覚言語モデルの成功は、それが７軸アームとグリッパーの組合せであっても、様々な分野で産業構造を変えると予想されている。
- その中で、3Dカメラと認識・計測技術が標準化されないものになってしまうと、使い勝手の悪さのために、なかったと同じものになってしまう。
- 使い勝手を向上させることが、新しい基盤になるうえで必要だ。

##### 今回のAIの進展が期待はずれとなりにくい理由
- かつて、AIへの期待が広がった時期が会って、期待されるほどには結果を出せずに、AIの冬の時期があった。
- 今回のすぐに冬の時代をむかえるだろうと思っている人もいるかもしれない。
- しかし、私は、今回の進展は一時的なブームではなく、継続的な進展になると確認している。
- 理由：
  - 部分的に役に立つタスクを様々にこなしていること
  - 制約の少ない条件でのタスクをこなしていること
    - 例：対象物を限定しない物体検出
    - 例：画像と言語の連携が進んでいること
    - 例：機械学習結果を利用して機械学習を改善するループが回りだしていること
  - カメラと自律マシンとの間で、外界についての「理解」を可能にしてきていること
  - 今回のAIの開発では、コミュニティの規模が大きくなっていることによる加速が生じていること

## 公開されることで開発が加速される。
#### コミュニティの規模が開発を加速させる。
- 「こういうのほしいよ」、「実装してみたよ」の発生するイベントの数は、コミュニティの規模Nの2乗に比例する。
- 「実装してみたよ」、「使ってみたよ」の発生するイベントの数は、コミュニティの規模Nの2乗に比例する。
- 「製品を販売します」、「購入します」の発生するイベントの数は、コミュニティの規模Nの2乗に比例する。
こういう状況において、英語圏・中国語圏のコミュニティにくらべ、日本語のコミュニティの規模は小さい。
それなのに、アイディアや、秘匿するまでもない実装を秘匿することは、圧倒的な遅れを引き起こす。
コミュニティを作る努力は、論文の調査の協力や、コミュニティのメンバー間の努力によってなされている。
3Dカメラと画像認識・視覚言語モデルなどのコミュニティが活発になることは次の効果をもたらすと考えている。

#### 僕たちの実現しようとする未来は、自部署だけじゃ作れない
- 僕たちが山ほど書いてきた実装のほとんどは、その時点では会社の売上に貢献したかもしれない。
- しかし、それらのコードは、その次の未来を築くことなく、古びて失われていった。
  - GitHub以前では、コードを確実に次の世代のメンバーに受け渡す方法がなかった。
  - 担当した人が職場からされば、その存在さえ忘れさられる。
  - Not Invented Here の価値観は、評価してみるよりも先に、自分で書き出すのをしがちだろう。
- 僕たちの作ろうとする未来において、３Ｄ計測と画像認識と視覚言語モデルの融合は、どこかの誰かによって支配されることを望まない。
  - 協力して未来を築かなければ、どこかの誰かに支配されて終わってしまう。
  - 支配された状況では全ての富は、その技術を支配する人たちに握られてしまう。
  - 支配された状況では、さらなる改良を推進できない。
- 自部署だけのノウハウとして秘匿する部分があっていい。
- それ以外の秘匿する価値のない部分は共有して、APIをそろえていこう。
- 秘匿するコードもAPI設計を共通にしておけば、商業的にサポートしているアルゴリズムの力を見せつけることが簡単だ。
- くり返し言う。僕たちの実現しようとする未来は、自部署だけじゃ作れない。

#### 効果
- 初心者が、その分野の最新の状況に入りやすい。
- 3Dカメラの選択、ボードの選択、ライブラリの選択などについての情報を得やすい。
- それぞれの方式が、どういう条件では効果的であり、どういう条件では使い物にならなくなるのかがわかりやすい。
- SOTAに準じるアルゴリズムを早い段階で利用できる。
- どういうAPI設計をすれば、ライブラリとして共有しやすいのかがわかる。



## 私たちにできること
- 利用している３Dカメラの違いを超えた３Dカメラ利用者のコミュニティの創出
- ある３Dカメラにある実装を、他の３Dカメラへの移植の可能性の共有
- 利用するOS、CPUボード、GPU、アクセラレータの違いを超えての情報の共有化
- 視覚言語モデルなどの進展を取り入れやすくすること


# design policy
1. アルゴリズムを置換え可能にする。
  - 入力出力が同じ形式のアルゴリズムは、置換え可能なようにインタフェースを設計する。
  - ちょうど、scikit-learnがそれぞれのアルゴリズムでfit(), predict()が共通の設計思想で実装されているのに近づける。
  - スレレオ画像からの視差もしくは深度を算出するメソッドのインタフェースは、アルゴリズムの種類を変えても共通にする。
2. 目的が同じ３Dカメラのインタフェースも、可能な範囲で置換え可能にする。
  - ３Dカメラを用いた上位のアプリケーションと、3Dカメラ自体の機能とはインタフェースで切り離した設計にする。
  - これによって、３Dカメラを用いた上位のアプリケーションを１から作り直すことを減らす。
3. アルゴリズムのための各種設定パラメータのデフォルト値
  - アルゴリズムごとに、必要な設定パラメータは異なる。
  - その設定パラメータにデフォルト値が用意されていること。
  - そのデフォルト値を使えば、それなりの動作をすること。
  - 理由：従来の実装によっては、全てのパラメータを明示的に与えないと使用できず、しかもそのアルゴリズムでの推奨のパラメータが明示されないことがある。
  - それが、利用者にとって使い勝手の悪さをもたらす。
4. 推論のメソッド
  - 推論のメソッドには、その実装で一番効率的なインタフェースを持つこと
  - 他に、np.ndarray型の画像で、どの実装でも利用できるメソッドも持つこと
5.　推論メソッドの戻り値
  - 利用目的が同一であれば、戻り値のデータ構造は極力同一であること。
  - 例：矩形検出枠での検出結果の戻り値
    - 差し替え可能なインタフェース：
      - 検出アルゴリズムのライブラリが変わっても、同一であること。
    - 学習用のライブラリとの整合性のよい戻り値のインタフェース：
      - 推論した結果は、機械学習の学習用に転用される場合もあります。
      - そのためにも、学習用のライブラリとの整合性のよい戻り値のインタフェースが必要になります。
    -　推論結果のファイルインタフェース：
      - 最近のアノテーションツールで利用されているデータ形式を検討しましょう。
      - 理由：
      - アノテーションツールの自作はしたくないため。
      - 標準化されているツールだと、評価結果の集計がしやすい。
6. 学習用・評価用のデータセットのデータ構造を気にする。
  - 着目しているタスク：　代表的なデータデータ・セット
  - そのデータセットで学習・評価するためのスクリプト
7. 表示系の機能を、個々のアルゴリズム・個々の3Dカメラの実装から切り離す。
  - 表示系の開発コストを削減する。
  - 画像ファイルでの確認用のインタフェース
  - リアルタイム表示用のインタフェース
  - 3D表示用のインタフェース 
7. アルゴリズム利用の際の結果データのデータ形式
  - ネットワーク間でそのデータを転送する際のインタフェース
8. それぞれのモジュールがカメラを接続しようとはしないこと
  - 実装例：モジュールじたいの中では,カメラのインスタンスを生成しない。mainの中でカメラのインスタンスを生成する。
  - 理由：１つのカメラに接続できるのは、１つのアプリケーションだけである。

## 実装の利用例

```.py:
    disparity_calculator = stereosgbm.DisparityCalculator(
        window_size=window_size, min_disp=min_disp, max_disp=max_disp
    )
	disparity = disparity_calculator.predict(left.copy(), right.copy())
```

別なステレオ計測での実行例の１部では、次のようになる。
どちらでも、似たコードになる。
アルゴリズムの違いによる設定パラメータの違いは、インスタンスを作成する際のパラメータの違いとして吸収される。
```.py:
      disparity_calculator = stereoigev.DisparityCalculator(args=args)
      disparity = disparity_calculator.predict(left.copy(), right.copy())
```

- どちらの場合も同じライブラリを用いて、
  - 視差画像の可視化ができる
  - 点群への変換ができる
  - 再投影画像を作れる

## class ベースの設計
設計の例
- インスタンスを生成する際に、最小限の設定をすれば、動作すること
- defaultでも動作できるようにしておく
- メソッドの実行の際には、画像を渡せば実行できるようにする。
- 推論のメソッドで公開するインタフェース
  - np.ndarray のインタフェース
  - その実装で、もっとも効果的なインタフェース

#### 後処理込みのAPI
推奨：機械学習を含む場合でも、ユーザーに公開するAPIにおいては、後処理込みのものとすること
非推奨： 深層学習後のネットワークの出力そのままを返すので、解釈可能にするためにユーザー側が後処理を別に実施する。

#### 注意点
- グローバル変数を極力残さない
- 意味のまとまりが、classにまとめられていない。

## 画像データの選択肢

- １：np.ndarray型 (numpy, opencv)
  - np.ndarray型の場合には、色順序がRGBなのかBGRなのかは明示する。
  - cv2.imread(filename)
  - skimage.io.imread(filename)
  - imageio.imread(filename)
  np.ndarray型では、画像の画素値を整数値で扱う場合もあるし、float型を使う場合もある。
  float型の使われ方としては、[0.0 - 1.0]の間の連続値として用いられている場合もある。
  skimageの方がfloat型の場合の対応されている範囲が広い。
- ２：PIL.Image型
  - Image.open(imfile)
- 3: open3d.geometry.Image型
  - open3d.io.read_image(filename)
- 4: Torch, TorchVision
Tensor 型では、データをCPUだけではなく、GPUを指定できる。
（そのため、処理をGPUで完結させたいときには、CPUとGPU間のデータ転送を発生させないようにコーディングするのがのぞましい。）
- torch.from_numpy()

- class torchvision.tv_tensors.Image(data: Any, *, dtype: Optional[dtype] = None, device: Optional[Union[device, str, int]] = None, requires_grad: Optional[bool] = None
https://pytorch.org/vision/main/generated/torchvision.tv_tensors.Image.html
- 5: tf.Tensor 型 Tensorflow
 GPU, TPUに対応している。

#### 商用ライブラリの独自データ形式
- 商用の画像認識ライブラリは、独自のデータ型を用いている場合がある。
- そして多くの場合は、np.ndarray型と相互変換できる補助関数を用意している。

#### サードパーティ依存の画像データの例
- 1: StereoLabs ZED SDK の　sl.Mat() 型
- 画像の取得時刻情報などを含んだデータ形式であり、get_data()でnp.ndarray型に変換できる。
- ただ、チャネル数が４になっていることに注意

```.py:
import pyzed.sl as sl
zed = sl.Camera()
image = sl.Mat()
zed.retrieve_image(image, sl.VIEW.LEFT, sl.MEM.CPU, display_resolution)
cv_img = image.get_data()
```

#### 画像認識・機械学習系にデータを渡す場合の注意
- チャネル数が、そのAPIで使用するものと一致していること
- 部分画像を渡したいときには `image[100:200, 300:400, :].copy()` などとして連続な領域として渡す

#### GPU上にその画像データはおけるのか
TensorFlowのTensor型ではデータは、CPU、GPUのどちらにも置ける。
しかし、大半の画像データ形式は、CPU上に限られる。


## 点群データの扱い
- 手法１：3DカメラのSDK依存のデータ構造とライブラリ
  - この場合だと3DカメラのSDKの種類ごとに、異なるデータ形式になる。
- 手法２：Open3D のデータ構造とライブラリ
　- Open3Dの場合、それぞれのデータが明示的に型設定されている。
　- そのため、定型的な操作は実装済みの関数・メソッドの呼び出しとなる。
　- 引数がどの型なのかによって、変数の意味が明解になる。
- 手法３：OpenCVやnumpyの範囲のデータ構造とライブラリ
  - OpenCVでは３次元のデータとしてむき出しのデータ構造になっている。
　- 線形代数を扱うにはちょうどよい。
　- 引数のデータの型がnp.ndarray 型になんでもなってしまうので、変数の意味合いがわかりにくい。
　- OpenCVそれ自体には３次元データを扱うための関数群が不足している。
　- そのため、Open3Dならば用意されている機能を各自が実装してしまう。
- 手法４：Unityなどの３Dのゲームエンジン
  - 3DカメラのSDKを利用して、点群データを取り込む

#### 点群データの扱いに対する私見
- OpenCVを使うよりは、Open3Dを使うほうがのぞましい
  - 理由：
  - OpenCVベースの自作ライブラリは負債になる。
  - 可読性が低い。間違えた使い方を見つけにくい。
  - 並列演算のプラットフォーム対応を、利用するライブラリのコミュニティに委ねたい。
  - たとえ１行で書ける関数であっても、テストなしの自作関数はバグの温床になる。

## test
ロボットに使われる画像計測・画像認識についてトレース可能なテストがほしい。
そのカメラとそのアルゴリズムに対して、どのようなデータでどのようなテストをしているのかが
トレースできる環境を構築する。

#### testの方針
- 個々のリポジトリごとにテストを行う。
- アルゴリズム自体のテストのためには、カメラが不要でテストする。
- テスト用に使用するデータは、誰でもがダウンロード可能にして、誰でもがテスト可能な状況を目指す。
- CircleCIなどでは対応していないボード・CPUの場合には、ローカルなデバイス上でテスト自動化を目指す。
###### センサなしでのテスト
画像計測アルゴリズム・画像認識アルゴリズムの単体テストは、センサなしでも実現できる。
入力データをファイルにしておいて、それを読んで動作させて、結果を期待値と比較することでテストが可能である。
画像データは、pngファイルなどの損失のないデータを用いる。
pythonの場合：npyファイル, npz ファイルを用いることで、numpyで表されるデータを保存できる。
それらを使うことで、センサなしで、アルゴリズムへの単体テストができる。

###### センサ特化での再現性試験
- センサとそのSDKによっては、センサデータを独自形式でファイルに保存できるものがある。
　それを使うと、カメラデバイスを外した状況で、あたかもカメラを接続したような動作ができる。
　そのことによって、同じセンサデータの時系列を与えたときの出力を、アルゴリズムの種類・パラメータの変更によってどうなるのかを比較できる。
　例： StereoLabs ZED SDK
  [recording/playback](https://github.com/stereolabs/zed-sdk/tree/master/recording/playback)

######  ROS2での再現試験
ros2bag という仕組みがあって、センサデータをほrecordしplayback する機能がある。
- [Recording and playing back data](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html)
- [Recording and playing back data](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html)

## implementation
#### python3の場合
```commandline
python3 -m pip install .
```
でモジュールをパッケージとしてインストールできるようにする。
あるいは、pypi に登録して
```commandline
python3 -m pip install パッケージ名
```
ができることを目指す。
そのため、モジュール名、パッケージ名は既存のpypi に登録してあるのと名前がぶつからないように付ける。

###### headless への対応
組み込み分野の画像計測・画像認識処理では、本番の運用の中では、画像表示を持たないことが多い。
そのため、アルゴリズムの実行の中では、画面表示をもつべきではない。
Linuxの場合にはX11の表示先を期待してはならない。
cv2.imshow()を必要としてはならない。

###### 標準出力
- アルゴリズムのモジュール自体は標準出力にモジュールはメッセージを表示すべきではない。
- 実装先のシステムが用意しているlogシステムに出力する。
- pythonの場合にはloggingがある。

## C++(C++11以降)の場合
なるべく最新のC++の規格を採用したコーディングを心がけましょう。
- [The OpenCV Coding Style Guide](https://github.com/opencv/opencv/wiki/Coding_Style_Guide)
- [Open3D style guide](https://www.open3d.org/docs/0.12.0/contribute/styleguide.html)
- [ROS2 coding style](https://docs.ros.org/en/rolling/The-ROS2-Project/Contributing/Code-Style-Language-Versions.html#id1)
 ROS2では`Rolling targets C++17.` と言っている。
 3Dカメラを利用したアルゴリズムもそれに従うのがよい。


CMakeを使ってライブラリの作成、サンプルの実行形式を作れるようにしましょう。

## data file format
#### データ形式は、極力標準的なデータ形式を採用する。
- CPUの種類(bit数、エンディアン）によらないデータ形式を用いる。
- 機械可読性が必要とされるテキストファイルの場合には、以下のようなコンピュータ言語によらず可読な標準的なデータ形式を用いる。
  - json
  - yaml
  - toml
- pythonの場合の数値データの保存
  - npy ファイルを用います。NaN, PosInf, NegInfなどの値を持つことができます。
- OpenEXR: ハイダイナミックレンジイメージのための画像ファイルフォーマット
- ply: 点群データのファイルフォーマット。Open3Dで読み書きできます。
#### 使わないデータ形式
pickle
 pythonのオブジェクトをほとんどなんでも保存できてしまうデータ形式です。
　そのため、loadしたpickle オブジェクトを含むなにかを実行した時に、何が起こるかわからないという致命的な欠陥があります。

## ライブラリの依存性
- 可能な範囲でOpenSourceで標準になりつつある実装に寄せていく。
- 商用実装の利用が避けられない場合でも、APIの実装は極力標準化するようにつとめる。

## コミュニティの国際化
- 有用な実装は、世界のどこでも利用できるべきです。
- 有用な実装は、英語の範囲でソースコードが記述され、英語のドキュメントを含んでいなければなりません。

