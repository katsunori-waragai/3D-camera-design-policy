# これからの３Dと画像認識の連携で必要となる実装（私見）
- 2Dの物体検出をOpen3Dベースの深度情報と関連付けられること
- 2Dのインスタンスセグメンテーションの結果をOpen3Dベースの深度情報と関連付けられること
  - インスタンスセグメンテーションで絞り込まれた領域から、深度分布のパーセンタイル値が算出できること
  - 例：３Dカメラとセグメンテーション
  - https://github.com/bowu1004/instance_segmentation_RealSense
  - https://github.com/stereolabs/zed-sdk/tree/master/object%20detection/custom%20detector/python/pytorch_yolov8_seg
- 物体検出の結果に対する追跡
  - ３Dとしての物体検出では、対象物の空間的位置が重なることが生じにくい。
  - 3Dでの物体の追跡は、2Dの場合よりも安定になりやすい。
  - 例：検出結果にtracking_IDがついています。
  - https://github.com/stereolabs/zed-sdk/tree/master/object%20detection/birds%20eye%20viewer/python
  - object detection/birds eye viewer/python$ python3 object_detection_birds_view.py
- 人検出・人のポーズ推定に関わるライブラリが3Dカメラと連携がとれること
  - UnityやUnrealEngine などの3Dゲームエンジンと連動できること。
  - これらのゲームエンジンに対して人物の３Dとしてのポーズを取得して連携できること。
  - 例：
  - https://github.com/stereolabs/zed-sdk/tree/master/body%20tracking
- 顔検出だけでなく、顔照合を3Dカメラで連携がとれること。
  - 3Dでの追跡ができることを利用して、顔照合の頻度は減らせること。
  - 顔照合は、利用可能なライブラリの情報を含める（商用ライブラリも含める）。
  - 動画生成の技術の進展のため、2Dでのカメラを用いた顔照合は、偽装の危機にさらされています。
  - ３Dカメラを用いた顔照合技術は、当面は偽装不可能な照合技術になるでしょう。
  - 例：[Intel RealSense ID for Facial Authentication 　Securely Unlock your World](https://www.intelrealsense.com/facial-authentication/)
- IMUを使って、世界座標系に対する点群の位置を算出できること

- 過去フレームのデータも使って、環境の点群・着目している対象物の点群を生成できること
  - 階段を昇る際に、常に足元を見続けるかどうか
  - 過去のフレームのデータを元に点群を生成して、その結果を保持していれば、足元の情報を元に、足の動かす量を算出できます。
  - 実装例：
  - [MonST3R: A Simple Approach for Estimating
Geometry in the Presence of Motion](https://monst3r-project.github.io/)
  - 移動しているカメラでみた動画から、環境の点群を生成できるだけではなく、他に移動している対象の動きも抽出している。それらが３Dとして解釈されているので、別の視点から動画を生成できている。
  - ロボットに組み込むためには、オーバースペックであるだろう。これよりも簡潔で計算量やメモリ消費量の少ない実装が求められるだろう。
## 生成系AIの進展を反映させたい実装
- 見えていない側の領域を補完する点群（あるいはメッシュ）の生成
- 人の動作：
  - 対象物の見えていない側の形状を暗黙に想定して動作を生成してる。
  - 缶コーヒーの裏側の奥行きを暗黙に仮定して動作を生成している。
  - 3D計測をステレオカメラで点群にしただけでは、ロボットの動作として足りない。
  - 仮説の生成・検証が動作のループの中にほしい。

## オーバーヘッドの少ない実装
- 複数の画像認識機能が必要になるとき、多数のライブラリを同時に起動することは減らしたい。
- なるべくなら、１つの実装で、従来の複数の実装を置換えたい。
- 複数のネットワーク構造で、推論を複数実行するよりは、単一のネットワークでの推論の方がいい。
- 例：
[DINO-X: A Unified Vision Model for Open-World Object Detection and Understanding](https://arxiv.org/abs/2411.14347)
- https://github.com/IDEA-Research/DINO-X-API

## 定型的な処理としてあってほしいもの
- 物体の大きさの計測
  - 例：段ボール箱の大きさの計測
  - 例：農産物の大きさによる分類
- 凸物体についての自動での画像分類
  - もしくは、caption(=説明文)の自動生成
  - 説明文の自動生成の方がのぞましいと考える。
  - 初見の状況に対応するためには、事前に検出対象のラベルのテキストプロンプトを用意するわけにはいかない。
  - 例：高速道路にブタが出現しても、対処できる必要がある。
- 背景と凸物体との分離
- 固定の配置の対象物とそれ以外の分離　 

# マシンビジョン分野での３Dカメラと画像認識
- 制御された環境での画像認識技術は、マシンビションと呼ばれることが多い。
- マシンビジョンでありがちな条件
  - カメラの位置が固定である。
  - 対象物の背景は制御できる。
  - 照明条件も制御できる。
- 制御された環境での利点
  - 差分画像を利用できる。
  - 対象物の見える領域を絞りこむことができる。
### マシンビジョン分野の３Ｄカメラ設定で許されること
- 固定されて使われるので、消費電力の制約が少なくなる。
- ステレオカメラ・structured light 方式のカメラの基線長を大きくすることで、深度の精度を向上させることができる。
  - ただし、その分コストがかかる。
- 処理にCUDAのあるPCを用いることができる。
- フレームレートの高いカメラを使うという設定が可能になる。

# 現状の3D計測・画像認識をベースとした移動するロボットへの私見
- 画像認識技術を用いたロボットが、従来は制御された環境だったのが、制御されていない環境に変化してきている。
- このことは、ロボットに対して求められる設計思想にも、変化を生じている。
- 従来：
  - 制御された環境において、制御された対象物に対して、決まりきった動作を行うためのビジョン技術だった。
- 現在進行中の分野：
  - 制御されていない環境で、変動の大きな対象物に対して、対象物の状況に応じた動作をすることが求められている。

## 制御されていない環境では、リピータビリティは意味が少ない。
- 制御されていない環境では、まったく同じ状況になることが少ないので、同じ動作を精度よく繰り返すことが意味が少ない。
- ペンダントを使って教示できるような状況ではない。
- １回の計測を高精度で行えばあとは対象物へのハンドリング開始を待てばよいという枠組みは、変動する環境では成り立ちません。
- 絶えず、今の状況確認しながら行動を修正し続けることが必要になってくる。
- その結果、1mm程度の位置精度（あるいはそれよりも高精度）を求めることは意味を持たなくなっている。

## 移動するロボットを想定することで、設計思想は変わる。
- ロボットの手首付近にカメラを設置する例が多い。
- アームの先端に近いほど、モーメントが大きくなってしまう。
- モーメントを小さくするには、アームの先端に付けるカメラは軽量であることが求められる。
  - 基線長が少ないカメラ、ToFカメラのうちコンパクトなもの、単眼カメラなどが将来的な製品での実装に求められるかもしれない。
- 移動するロボットにおいて、ロボットのアームに剛性を高めることは得策じゃない。
- 柵の中でだったら、位置制御による動作が許されるが、柵の外では、ぶつからないこと、ぶつかっても衝撃が少ないことが求められる。
- そのため制御方式も制御のために必要になるセンサ類も変わってくる。

## 身体性のあり方と制御とセンシングは密接に関係している。
- 動作速度が速ければ、それに見合う分だけセンシングも応答時間が短いことが必要となる。
- 足場の悪いところを2足歩行させようとすると、それに応じてセンシングと制御とが対応できることが必要になる。
- どちらか一方だけの変更で対応できることはすくない。
- 画像認識は、センシングの一部にすぎない。
- IMUやタッチセンサとの連動の中で、画像認識（とプラスアルファ）の学習を習得させる。

なぜ、一介のビジョンエンジニアがロボット自体の仕様に口出しするのか

理由：どんな目的でも汎用に使えるロボットビジョンの実装は存在しないから。

生物を考えてみよう。
それぞれの生物は、それぞれの視覚を持っている。
そして、それらの視覚が共通のレイアウトになっているわけではない。

・眼の配置
・眼の大きさ
・眼の視野の配置
・眼球が回転できるかどうか
・応答できる視覚の時定数
・眼で感知できる周波数領域（4色性色覚、３色性色覚、２色性色覚）


これらは、それぞれの生物の形態や行動様式に密接に関わっている。
・眼球の大きさ：
　その生物の個体の大きさに関わっている。
　また、その生物にとって視覚にどれだけリソースを割り振ることができるかにも関係している。
　パンダの場合、同じ程度の体重の哺乳動物に比べて小さな眼球をしている。
・眼の視野の配置
　後ろから食われる可能性が高い生物の場合、死角はまさしく生存に関わる。
　生物の進化の中で、眼はコストが高いものであるらしく、２眼から、さらに多くするようにはならなかった。
　だから、２眼の中で、できるだけ後方まで見えるような配置をしている生物が多い。
　両眼視をできる動物の方が偉いという価値判断は、人間の思い込みにすぎない。
　後ろから高速に近づいてすぐそばを通り過ぎていく自転車にさえ、私たちの視覚は対応できていない。



## 生物にとっての視覚が多様であるように、ロボットにとっての視覚も多様であるしかない。

だから、ビジョンのエンジニアもロボットの全体仕様にたいして意見する。

## 制御は、ビジョンに対しての要求水準を決める。
- 制御の周期は、ビジョンに対する応答速度の水準を決める。
- 例：カタツムリの動作の速度は、それ以上の応答速度を無意味にする。
　だから、カタツムリはカタツムリにとって必要でかつ、養いうる範囲のなかで視覚を作っている。
- 例：飛翔する昆虫のように、羽ばたきの制御の周波数が高い場合には、それに応じて高速な姿勢推定に対応できる視覚系が必要になっている。またそれを実現しているからこおおおおおそ、高速な飛翔が可能になっている。

だから、そのロボットがどのような時定数で制御を行うのかによって、最適な視覚の時定数も変わってくる。
　視覚系のオーバースペックは、ロボットにとって、計算リソースを他の用途で使うことを不可能にする。バッテリーのもちも悪くする。

## 動作性能は制御と密接
- 動作性能は、制御と密接に関わっている。
- 制御がない動作は、ただの暴走だ。

## タスクの目標に関係しない、ハードウェア設計・制御・センシングは無意味だ。
- 「ロボットのアームの剛性が低い」ということと、タスクを実現できているかということとは直結しない。
- タスクを実現するものでああれば、どちらでもOKなはずだ。
- タスクを実現するもののうち、必要なリソースが少なくて済むほうがいい。
- 「汎用人工知能をもって人並みの器用さと運動性能」に憧れるのは勝手だ。しかし、ビジネスは別だ。
  - ビジネスで考える限り、経済的な合理性が重要になる。
  - 他に十分に安いロボットが実現可能な時に、「汎用人工知能をもって人並みの器用さと運動性能」のロボットが市場に受け入れられる可能性はない。

## 起こりうる悪夢
- 人を安い道具として使い潰してしまって、移行のための準備期間をなくしてしまうこと。
- 安い労働力が確保できなくなった時点で、そのビジネスが終了してしまうこと。
- 安い労働力が確保できなくなったときには、ロボットで作業を置き換えるための投資をする体力も既になくなってしまっている。



## 時刻情報の整合性の重要性
・動きのある世界において、時刻情報は重要だ。
　ある瞬間の画像を撮影しても、その瞬間とは30ms 程度の時間幅をもつかもしれない。
　このタイミングのデータを利用すると決めた時に、そのデータに時刻情報を付ける。
　複数のセンサデータに同じタイミングで時刻情報を付ける。
　データを3Dとして取得する。
　この部分は、3D化のための処理に時間がかかる。
　並行処理で、時刻情報のタグを付けたデータ処理を実行する。
