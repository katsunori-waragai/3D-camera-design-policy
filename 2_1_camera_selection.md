# 3Dカメラの選択

以下のようにさまざまな３Dカメラがあるが、比較的簡単に置換え可能なことがのぞましい。

## カメラの種類
- Stereo Camera
- active stereo Camera
- ToF Camera
- LiDAR ＆ camera
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

### 3D Cameraの使えるボードコンピュータ、OSには制約がある。
ほとんどの３Dカメラの場合、接続できるボードコンピュータ、OSには制約がある。
#### ZED SDK(StereoLabs)
- [x] Windows 10/11
- [x] Ubuntu 20/22/24
いずれもCUDAデバイスがあること。
対象外
- macOS
- AndroidOS


#### Intel RealSense
[Intel RealSense SDK 2.0](https://www.intelrealsense.com/sdk-2/) はサポートしているプラットフォームが広い。
- [x] Windows
- [x] Linux
- [x] AndroidOS
- [x] MacOS

#### Orbbec
[Orbbec SDK](https://www.orbbec.com/developers/orbbec-sdk/)
- [x] Windows
- [x] Linux
- [x] AndroidOS
- [x] MacOS

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
### 単眼カメラでの好ましい特性のカメラ
Tier IV C1 Camera
[自動運転&モビリティ向け車載HDRカメラ](https://www.paltek.co.jp/solution/tier4/index.html)


## 複数台の3Dカメラ
- 用途においては、一つのシステムの中に複数台の3Dカメラが入ってくる。
- [ ] システムは、その複数台の3Dカメラを接続できますか。帯域が足りてますか、電源が足りますか。
- [ ] システムは、その複数台の3Dカメラをシリアル番号などで区別できますか。
- [ ] その3Dカメラは、複数台で干渉しませんか。あるToFカメラの照射光による反射を別なToFカメラで受光して、結果がおかしくなるなんてことはありませんか。
- [ ] 自律走行台車のような分野で、360度をカバーするには、何台の3Dカメラを必要としますか。

### 複数台3Dカメラの利用例
- StereoLabs
  - https://www.stereolabs.com/en-jp/our-technology
  - https://www.stereolabs.com/en-jp/products/zed-box
- RealSense
  - [Multi-Camera configurations - D400 Series Stereo Cameras](https://dev.intelrealsense.com/docs/multiple-depth-cameras-configuration)
- Orbbec
  - [Multi-Camera Sync Hub Dev/Pro ](https://www.orbbec.com/staging/wp-content/uploads/2023/08/ORBBEC_Datasheet_Multi-Camera-Sync-Hub-0816-v01.pdf) 


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
- 耐水性・防塵性のあるもの
  - 汎用のロボットを目指す場合、しずくがとんだくらいで、壊れてほしくない。
  - IP66　の防塵防水機能があるものを選ぶと良い。

#### 利用するアプリケーション側が明確に決まっている場合
- 例： Unityで使うことが決まっている。
- その場合には、Unityに対応している3Dカメラを使うのが一番。
- 未対応の3Dカメラを使えるように対応付けるのは、簡単ではない。
- 例：OmniVerse などのデジタルツィンの中で使う場合
- これも、NVIDIAのサイトなどを確認して対応済みの3Dカメラを使うのがよい。


## active stereo and structured light
[Structured-light-stereo](https://github.com/feiran-l/Structured-light-stereo)
stereocamera が特徴をつかみにくい領域で、視差の計算をすると、結果が不安定になってしまう。
プロジェクタと組み合わせることで、structured light　を実現できる。
active stereo カメラは、パターンを投影して、それをステレオ計測するのを近赤外光で実行しているものです。

#### Structured light
- https://qiita.com/nonbiri15/private/f88312b2a2f25dd85d68
- https://kushalvyas.github.io/sl_3d.html
- [OpenCVのstructured-lightを流用して3Dスキャン](https://qiita.com/nn_tok/items/b2233a5632ec9fcaf714)

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


## 3Dカメラの方式における私見
### ToFカメラは、ToFカメラで十分であることが確認されたときだけ、ToFカメラだけという選択ができる。
- 理由：
  - ToFは、以下のような弱点を持つ
  - 遠方の検出限界が数ｍである。
  - 黒い物体（特に近赤外領域で吸収のある場合）は、実際よりも遠く見える。
  - 特に近赤外領域で吸収のある場合には、50cmの距離にあっても検出不能の場合がある。このため50cm先の歩行者をToFでは障害物検出できない場合がある。
  - 鏡のように鏡面反射成分が強いには、鏡面反射で戻ってきた先の位置を返す。このため、光沢のある冷蔵のパネルなどでは、正しい距離を返さないことがある。
  - 全ての画像センサに言えることだが、透明物体に対して、適切な深度を返すことができない。
- 対策：
  - 近距離で接触・衝突の可能性がある場合には、超音波センサと組み合わせることで、接触・衝突の危険を減らすことができる。
- ToFカメラの範囲での可能性
  - 深度画像の他に、近赤外のモノクロ画像が得られる。モノクロ画像で物体検出し、その結果を深度画像と組み合わせるという用途がある。
  - 目的によっては、それで十分かもしれない。

#### ステレオカメラは、ステレオカメラの弱点を知って使ってね
- 理由：
  - 元画像が不十分品質ならば、既に終わっている。
  - 利用条件が設置者が条件を整えられる状況のとき
    - それなりにうまくいきやすいです・
  - 利用条件が悪条件を含むとき
    - カメラは、白飛び・黒つぶれ・SN比の低下、直射光の入射などの様々な要因で画質が劣化する。
    - そのときに、要求仕様を満足できるかだ。
    - テクスチャーがない領域、黒い領域、周期的パターンが繰り返している状況において、ステレオ計測に失敗することがある。
    - 透明な物体、透明なコップなどで、大外しをすることがある。
- 安全性を高める対策を必ずお願い
  - 近距離で接触・衝突の可能性がある場合には、超音波センサと組み合わせることで、接触・衝突の危険を減らすことができる。
  - 窓ガラスなどにぶつからないようにするにも、超音波センサと組み合わせることで、接触・衝突の危険を減らすことができる。
- 制御された状況では
  - そのような悪条件にならない制御された環境では、ステレオカメラで十分なタスクが多い。
  - 偏光フィルタ付きでかつSDKが充実しているステレオカメラを選択するのがよい。

#### Active stereo camera は、特性が安定する。
- ３つの方式の中では、一番破綻しにくいロジックになっている。
- そのことが、充実したSDKのあるカメラでの利用が多い理由だとおもう。
- ステレオカメラと違って、点群の算出部分に工夫の余地がないので、3Dカメラ以外の本業に集中できるというメリットもある。
（ああ、これはToFも同じか。）
- 注意点：Active stereo cameraとRGBカメラの視点の位置が違うということ
近距離のデータを扱う場合には点群を平行移動して解釈させるということ。
- 対策案：Active stereo cameraの点群データを、RGBカメラの位置に再投影してRGB画像への対応するdepthを算出できるのではないかと期待する。
その際に、Active stereo cameraでの計測に用いているカメラパラメータと、RGBカメラのパラメータが違いことに対する扱いを適切にすること。
Open3Dのようなライブラリで、そのような操作ができると思う。

###### 弱点
- 他の3Dカメラと同様に、以下の場合には算出できない。
- 鏡・透明な物体は、光がそれぞれ、反射先・透過先の計測をしてしまうという限界がある。
対策
- 移動ロボットなどの場合には、超音波センサと組み合わせることで、接触・衝突の危険を減らすことができる。
- 利用しているカメラの製造が打ち切られないことが大事。
- 別の会社の提供している代替品に乗り換えられる程度に両方を知っておく必要がある。

## 複数方式の併用型の3Dカメラ
ToFカメラは、圧倒的に弱点が多い。そこで、ToFカメラと別の方式とを組合せた3Dカメラが市販されている。
### 例：ToFカメラとグレースケール広角ステレオカメラとの併用
[XVISIO](https://www.xvisiotech.com/product-center/)

## jsonファイルでのカメラ情報
[stereo_camera.json](stereo_camera.json)


## LiDARについて
製品例
https://www.xgrids.com/lixell2pro

LiDARは十年前よりも格段に値段が低下している。
予算がゆるせば、検討する候補に入れていいだろう。

LiDARの圧倒的な強みは、遠方への距離の正確さだろう。
[LiDAR（ライダー）とは？これだけは知っておきたい３つのこと](https://jp.mathworks.com/discovery/lidar.html)
この記事に中で、代表的なLiDARのメーカーのリストがある。

- Velodyne Lidar
- SICK
- 北陽電機
- Ouster
- Ibeo Automotive Systems 
- Livox

大きな部屋の端から端までの距離を正確に求める必要があるときには、ステレオ計測やSLAMなどの手法は期待できない。誤差が累積するからだ。

LiDARについて調べるには、自動運転関連の書籍・記事をあたるといいだろう。
例
- [自動運転のためのセンサシステム入門　車載カメラとＬｉＤＡＲによるセンサフュージョン](https://pub.nikkan.co.jp/book/b10021618.html)
- [自動運転技術入門　AI×ロボティクスによる自動車の進化](https://www.ohmsha.co.jp/book/9784274227011/)

## 対象物の速度が重要な場合
- Doppler効果でその瞬間の速度（相対速度）が求まるのはミリ波レーダーです。
- ステレオ計測では遠方の距離の精度がでないため、速度を算出するには適しません。


## SDKがしょぼくてもなんとかするという猛者の場合
- まず、どういう方式で通信するのかを確かめてくれ。
- CPUボード側に、どういうコネクタインタフェースがあればいいのか確認する。
  - CPUボード側からどのように電源供給するのか
  - 制御信号はどうなっているのか。
  - レジスタマップなども必要になるはずだ。
- デバイスドライバをどう書くのかを調べてくれ。
- 画像サイズとfps、遅延などがどうなるのかを確認してくれ。
- カメラパラメータの算出方法をなどを調べて、確かめてくれ。
- どういう条件で精度が劣化しやすいか考え、ハードウェアの対策をとってくれ。
- 独自のSDKを作って拡張する場合には、よく使われている3Dカメラの機能を参考にAPIを設計するといいだろう。
- 

## 単眼カメラ
ハイダイナミックレンジの単眼カメラ
- [Tier IV　C1 カメラ](https://edge.auto/automotive-camera)
- [StereoLabs ZED X One カメラ](https://www.stereolabs.com/en-jp/products/zed-x-one)
- Spresense カメラ
- [SPRESENSE HDRカメラボード[CXD5602PWBCAM2W]](https://www.switch-science.com/products/8080)
- [SPRESENSEカメラボード [CXD5602PWBCAM1]](https://www.switch-science.com/products/4119)

## 参考情報
- [市販ＴoＦカメラについて調査中(2023年版）](https://qiita.com/nonbiri15/items/5fdb9962bbdc76274d90)
- [ステレオカメラ調査(2023年版）](https://qiita.com/nonbiri15/items/a8590a99941f8de4c8b8)
- [ToFカメラについての重箱の隅](https://qiita.com/nonbiri15/items/064bbce3a2347f307119)
- [ステレオカメラの重箱の隅](https://qiita.com/nonbiri15/items/e0889c135acd90ae1319)

## カメラレビュー
- [We compared 8 types of 3D cameras in various environments [No. XNUMX indoor edition]](https://www.tegakari.net/en/2020/09/3dcamera_compare_2/)
- [Depth Accuracy Comparison: Luxonis OAK vs StereoLab™ ZED vs Intel® RealSense™](https://discuss.luxonis.com/blog/3734-depth-accuracy-comparison-luxonis-oak-vs-stereolab-zed-vs-intel-realsense)
- [3Dカメラ 8種類を 様々な環境で比較しました 【その① 屋外編】](https://www.tegakari.net/2020/09/3dcamera_compare_1/)
- [3Dカメラ 8種類を 様々な環境で比較しました 【その② 屋内編】](https://www.tegakari.net/2020/09/3dcamera_compare_2/)
- [ステレオカメラ業界を追放されし者のカメラレビュー（Gemini 335L）](https://qiita.com/Takumi3Dcamera/items/6d44e52c4a302da94de8)
- [ステレオカメラ業界を追放されし者のカメラレビュー（Femto Bolt ）](https://qiita.com/Takumi3Dcamera/items/45feb1f2ee1ddeb22991)
- [A Quick Comparison of the Orbbec and RealSense 3D Cameras](https://opencv.org/blog/a-quick-comparison-of-the-orbbec-and-realsense-3d-cameras/)


## 特殊なカメラ
- [イベントカメラの話](https://qiita.com/kminoda/items/6d71029f6c860da60740)
- [イベントベースビジョンセンサー（EVS）技術](https://www.sony-semicon.com/ja/technology/industry/evs.html)
- [イベントカメラ DAVIS346／DVXplorer Micro](https://nanoxeed.co.jp/product/eventcamera/)
- [イベントカメラの出力をもとに少ない計算量で高速に動きを認識 [CVPR2023]](https://www.d-itlab.co.jp/blog/2024/04/05/CVPR2023.html)
