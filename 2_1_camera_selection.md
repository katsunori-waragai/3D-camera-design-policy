# 3Dカメラ

以下のようにさまざまな３Dカメラがあるが、比較的簡単に置換え可能なことがのぞましい。

## カメラの種類
- Stereo Camera
- ToF Camera
- Structured light Camera
- monocular camera　(exceptional)

## camera hardware requirement
- 全ての時刻情報はCPUボード、各種センサボードで同期されていること
- 各種センサからの取得データには、時刻情報が付随していること。
- 画像センシングの結果には、必ず元となった画像データの取得時刻の情報を含んでいる
- データのスナップショットをとる操作と、そのデータを転送する操作とは区別されるべきである

#### あればなおうれしい項目
- 複数センサのデータのスナップショットを同期してとれれば、うれしい。
- 複数センサのデータの統合処理が楽になる。

---

# depth range and Field of View
3D カメラで計測可能なdepthと視野角とがある。

これは、３Dカメラを選んだときに決まる。
これによって、３Dカメラの候補のリストから自分たちの目的のためのカメラを選択する。

# camera connection
3D カメラは、ボードに接続するには、それぞれのインタフェースがある。

- USB
- MIPI(Camera Serial Interface)
- GMSL2
- その他多数

インタフェースによって、可能なケーブル長が異なる。
用途によっては、カメラの選択肢に影響する。


## 3D camera SDK
3Dカメラによっては、接続可能なCPUボードが予め制約されている。
そのOS用にカメラのデバイスドライバーが用意されている必要がある。
  自作カメラモジュールの場合には、そのデバイスドライバーを自身で開発する。
３Dカメラの場合、たいがい、OSと言語に対してSDKが用意されている。
そのSDKがあってはじめて、３Dカメラモジュールが標準で用意している深度・点群データが利用可能になる。
SDKとサンプル実装によっては、検出結果を３Dに表示して、追跡をするなどの例がある。
それらのSDKの充実度合いも、カメラを選択する際の考慮対象になる。

## digital twin
ロボットの動作を、シミュレーションによる強化学習を実施することが増えている。
それを実行するためには、シミュレーション空間上で、対象物の位置と姿勢を取得する必要がある。
単純な実装では、３D空間にある真値を利用するシミュレーションも存在する。
ただ実際の性能の予測には、センサ信号のシミュレーション、センサ信号にもとづく３D処理、画像認識処理を
実行するのがふつうである。
３D空間でのセンサ信号のシミュレーションを可能にするモジュールは、
３Dカメラメーカーによっては提供されている。


## カメラのノウハウ
- 偏光フィルタの有無
照明による反射の輝点は、ステレオ計測において問題となる。
偏光フィルタが利用可能なステレオカメラの場合、偏光フィルタ付きを選ぶのがよい。
- ToFカメラでの波長の選択
大気中の水蒸気による吸収がある波長域を用いているToFカメラを用いると、
屋外でも利用可能なToFカメラとなる。

## カメラ自体の特性
以下のような特性がRGBカメラ・グレースケールカメラでも重要になる。
- ハイダイナミックレンジ(HDR)
- 高感度・低ノイズ
- LEDフリッカー抑制

## selection by end user
ボードを自作しない範囲の人については、次の条件を満たすカメラを選択するとよい。
- カメラのゲイン設定をオートで対応してくれるカメラ
  - 画像計測・画像認識は、画像の品質が保たれていることが前提です。
  - オートのモードで動作させた時に、白飛びや黒つぶれが生じにくいことが大事です。
  - イメージセンサの品番が同じでも、イメージセンサのダイナミックレンジが保てるようにするには、ISPの実装次第です。
  - ステレオ計測の場合、黒つぶれも白飛びも、３D計測の性能がでなくしてしまいます。
- timestampを扱ってくれるカメラ
  - そのデータは、いつのタイミングに取得したものですか。物体検出などが済んだ時点で、それは「今じゃなくて過去」です。それがどのくらい過去なのかを知るには、 timestampが必須です。
  - 利用するシステムの中にある別のタイマーと時刻が一致している必要があります。
- カメラにIMUがあって、画像に同期したタイミングでの姿勢データが得られること
  - [ROS 2でRealSenseの内蔵IMUを使用する](https://qiita.com/porizou1/items/6ecc89d218502ad6fe68)
  - [ZED SDK のセンサーデータ](https://qiita.com/nonbiri15/items/0d3d5e5084f7318e8c69)
  - IMU内蔵の３Dカメラの取得する点群データは、カメラ座標系だけではなく、世界座標系で得られるタイプの3Dカメラがある。
  - 世界座標系で得られる場合、対象物が何ｍ先にあるのかを知ることができる。
- カメラ特性の変化が無視できること 
  - 落としてしまうなどの衝撃を加えても特性の変化が無視できること
    - ロボットなどの使い方で、その3Dカメラに衝撃が加わってしまうことは避けられません。
  - 使用する温度範囲の中で、深度を算出する特性への変化がないこと
    - 温度変化が加わると伸縮などの熱変形が生じます。カメラ間の基線長の変化だけではなく、それぞれのカメラの向きが変わることで、ステレオ計算の結果が著しく異なってしまいます。
    - 車載の分野で利用するならば、-10℃より低い温度や５０℃を超えるほど高温になる状況が考えられます。駆動していない状況での温度の履歴だと、-20℃や60℃以上の状況も想定しなければならないことのありえます。
    - ほんとうに必要な温度範囲を明らかにしてもらって、その範囲で品質保証のできる機構系の設計を求めることが必要になるでしょう。
    - もしくは、キャリブレーションによって簡単に対処できること
  - 使用する製品の寿命において、 深度を算出する特性への変化がないこと
    - その3Dカメラを組み込んだ機器の寿命は１０年を超えるかもしれません。そういう長期間に及んでも、３D計測の性能が維持できることが大事です。 
    - もしくは、キャリブレーションによって簡単に対処できること


## structured light
[Structured-light-stereo](https://github.com/feiran-l/Structured-light-stereo)
stereocamera が特徴をつかみにくい領域で、視差の計算をすると、結果が不安定になってしまう。
プロジェクタと組み合わせることで、structured light　を実現できる。

## カメラキャリブレーションについての記事
https://kushalvyas.github.io/calib.html

## ステレオカメラの種々のサポートレベル
- レンズとイメージセンサがある
  - デバイスドライブは各自作ってくれというレベル
  - カメラの固定と、温度に対する安定性は自分でやってくれ
  - ステレオ平行化のためのカメラパラメータの算出も自分でやってね。
- レンズとイメージセンサとデバイスドライバがあるだけのレベル
  - 視差の計算・深度の計算は勝手にやってくれというレベル 
- レンズとイメージセンサとデバイスドライバがあり、視差と深度の計算までは対応しているレベル
  - 点群への処理は自分でやってくれというレベル
-　レンズとイメージセンサとデバイスドライバがあり、視差と深度の計算し、点群への変換も含んでいるレベル
  - IMUやタイムスタンプをサポートしていることがある。
  - IMUを考慮後の世界座標での点群が得られる。
-　上記のレベルを満たしたうえで、物体検出との連携があり、追跡も３次元で行われるレベル。
  - そのようなサンプルコードがgithub上に公開されている。

#### ステレオ計算とアクセラレータ
ステレオカメラの種類によっては、深度計算の部分を行う機能をUSBデバイスの中に最初から含んでいるものもある。
独自のデバイスを含んでいるもの
https://shop.luxonis.com/products/oak-d
https://www.xvisiotech.com/product/seersense-ds80-st/
https://www.orbbec.com/products/stereo-vision-camera/gemini-336/

CUDAデバイスを前提としているもの
https://www.stereolabs.com/en-jp/store/products/zed-2i



## 3Dカメラ選択における私見
- サポートが十分になされているメーカーの製品を使おう。
- SDKがしっかりしていて、サンプルアプリケーションも充実しているのを選ぼう。
- Digital twinやシミュレーション空間における強化学習を行う際には、カメラの特性をシミュレーションできることが重要になる。
  - そこまでの対応を行なっているカメラはとても少ない。
  - https://docs.omniverse.nvidia.com/isaacsim/latest/features/environment_setup/assets/usd_assets_sensors.html#camera-and-depth-sensors
    - Intel RealSense D455, Orbbec Gemini 2, StereoLabs ZED X などがある。
  - https://docs.omniverse.nvidia.com/isaacsim/latest/features/sensors_simulation/isaac_sim_sensors_camera.html
  - https://docs.omniverse.nvidia.com/materials-and-rendering/latest/cameras.html

## jsonファイルでのカメラ情報
[stereo_camera.json](stereo_camera.json)

## 参考情報
- [市販ＴoＦカメラについて調査中(2023年版）](https://qiita.com/nonbiri15/items/5fdb9962bbdc76274d90)
- [ステレオカメラ調査(2023年版）](https://qiita.com/nonbiri15/items/a8590a99941f8de4c8b8)
- [ToFカメラについての重箱の隅](https://qiita.com/nonbiri15/items/064bbce3a2347f307119)
- [ステレオカメラの重箱の隅](https://qiita.com/nonbiri15/items/e0889c135acd90ae1319)

## カメラレビュー
- [We compared 8 types of 3D cameras in various environments [No. XNUMX indoor edition]](https://www.tegakari.net/en/2020/09/3dcamera_compare_2/)
- [Depth Accuracy Comparison: Luxonis OAK vs StereoLab™ ZED vs Intel® RealSense™](https://discuss.luxonis.com/blog/3734-depth-accuracy-comparison-luxonis-oak-vs-stereolab-zed-vs-intel-realsense)
- [ステレオカメラ業界を追放されし者のカメラレビュー（Gemini 335L）](https://qiita.com/Takumi3Dcamera/items/6d44e52c4a302da94de8)
