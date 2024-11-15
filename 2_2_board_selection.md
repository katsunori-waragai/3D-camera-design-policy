# CPUボードの選択
PCやMacを使うのでなければ、ボードの選択が必要になる。
(PCの場合にはGPUのボードということもある。)
MacでもPyTorchが利用できるらしい。

## シングルボードコンピュータ系
### NVIDIA Jetson AGX Orin
シングルボードコンピュータとして、NVIDIA Jetson AGX Orinを使うことができます。

SOM　Jetson Orin NX, Jeston Orin Nano

大半の機械学習のフレームワークが動作する。

### Raspberry Pi
raspberry pi 5 

##### raspberry pi 5+「Raspberry Pi AI Kit」
[ラズパイ5にAIキットが登場、処理性能は13TOPS](https://monoist.itmedia.co.jp/mn/articles/2406/05/news086.html)

[Raspberry Pi AI HAT+（26TOPS版）](https://www.switch-science.com/products/9999?srsltid=AfmBOopiZbk5tXGoCWj10eUNKbUeegZq9smK_GoK_DjphtTRUAyFLrsJ)
> Hailo 8 AIアクセラレーターを搭載したRaspberry Pi 5用のHAT+です。Raspberry Pi 5（4 GB / 8 GB）上で優れた推論性能（最大26 TOPS）を発揮させることができます。13 TOPSの廉価版も取り扱いがあります。



### Qualcomm
[Qualcomm Neural Processing SDK for AI](https://www.qualcomm.com/developer/software/neural-processing-sdk-for-ai)


### D-Robotics
[D-Robotics RDK X3](https://www.switch-science.com/products/9836)
5 TOPS（Tera Operations per Second）の推論演算能力を備えた開発ボードです。多様なセンサや、その他の拡張ハードウェア、接続方法に対応しています。


### SONY SPRESENSE
[SPRESENSE](https://www.sony-semicon.com/ja/products/spresense/index.html)
[Neural Network Console](https://dl.sony.com/ja/)
【Neural Network Consoleクラウド版サービス終了のお知らせ】
Neural Network Console クラウド版は2024年12月25日をもってサービスを終了させて頂きます。長い間ご愛顧頂きありがとうございました。
なお有償の法人向けプランをご利用頂いているお客様向けには別途終了時期のご連絡をさせて頂きます。
またNeural Network Console Windows版は提供を継続いたしますので引き続きご活用ください。

### Renesas　kakip
[kakip](https://www.kakip.ai/)

## アクセラレータ系
アクセラレータ系は、接続可能なボードの種類（CPU, OS, インタフェース）などを確認すること。
画像データのような大量のデータの扱いがどうなるかを調べること。
なかには、３Dカメラの中にはMovidius などのデバイスを予めもっているものもある。
### Hailo
[Hailo-8 AI Accelerator](https://hailo.ai/ja/products/ai-accelerators/hailo-8-ai-accelerator/)

[Hailo-8 Edge AI Accelerator Deployment Guide](https://tlab.hongo.wide.ad.jp/2024/03/04/hailo-8-edge-ai-accelerator-deployment-guide/)

HailoModelZoo/docs/GETTING_STARTED.rst
https://github.com/hailo-ai/hailo_model_zoo/blob/master/docs/GETTING_STARTED.rst


[ﾗｽﾞﾊﾟｲ5・RealSense・Hailo-8でセグメンテーションタスク推論中の消費電力：7.5W
すごく省電力](https://x.com/Ray255Ar/status/1798686519571669354)


Hailo-8 がRaspberry Pi AI HAT　として利用可能になっているので、RaspberryPI 5と組合で利用するのがいいだろう。

### Coral USB Accelerator
[Coral USB Accelerator](https://coral.ai/products/accelerator/)

### VPU
[Movidius™ ビジョン・プロセシング・ユニット (VPU)](https://www.intel.co.jp/content/www/jp/ja/products/details/processors/movidius-vpu.html)
Movidius Neural Compute Stick (NCS),


## ボード選択のポイント
まずは、Raspberry Pi 5や、Jetson AGX Orin などのボードの仕様から確認しよう。

- CPU, GPUなどのアクセラレータ, OS
  - 演算性能が足りているか
  - 起動しっぱなしになるプロセス・スレッドの数に対して、コア数が十分か
- インタフェース
  - カメラインタフェース
    - CSI(MIPI）：　例 Raspberry Pi 5では 2台のカメラが接続可能になっている。
    - Jetson AGX Orin Industrialの場合だと、`Up to 6 cameras ` との記載がある。
  - USBインタフェースの数
    - USBカメラから、3Dカメラへの給電をしているのが大半であり、複数のデバイスに給電していると、ボードによっては給電能力が不足する場合があることに注意。
  - ネットワーク性能
  - wifi の対応の有無
  - GPIO
-　ロボティクス分野で必要になるインタフェース
  - 利用する製品分野によっては、使用するインターフェースが決められている。
  - [「OPC UA for Robotics」で解決したい課題](https://monoist.itmedia.co.jp/mn/articles/2003/25/news006_2.html)の記事に見る限り、とても標準化されていない。
  - 画像認識屋は、[PLC](https://www.fa.omron.co.jp/product/special/library/robotics/plc/)など知らない。
  - [Ethercat](https://www.macnica.co.jp/business/semiconductor/articles/texas_instruments/125025/)も知らない。
  - 
- 開発のしやすさ
  - ネットワーク必須（機械学習・画像認識では、ネットワークなしの開発は考えられない）
  - HDMIポート, DisplayPort
  - 開発時のメモリの潤沢さ
  - 開発時のディスク領域の潤沢さ
  - ボード上でコンパイルができるのか （あるいは、クロスコンパイルが別のLinuxマシンからできるのか）

- 3Dカメラ対応
  - 3Dカメラを選択した時点で、可能なボードは限られる。
  - 3Dカメラのデバイスドライバー、3Dカメラを用いたSDKの時点で、可能なボードとOSと言語の組合せは限定される。
  - 例：StereoLabs　ZED2i: Windows10/11, Ubuntu PC+ Cuda GPU. Ubuntu Jetson 
  - 例:RealSense: Windows, Linux, Android, macOS 

## カスタムボード
- カメラの数を増やす、カメラインタフェースを追加するなどの状況では、開発キットにあるボードコンピュータでは適さなくなります。
- そのボードのSystem on Module (SOM) を利用しつつ、それと組み合わせるカスタムボードが必要になります。
- これらは、画像認識屋・機械学習屋の領域を超える範囲の作業です。
- しかるべき人に対応してもらいましょう。
- GMSL2のカメラを接続することを選んだときには、そのカメラインタフェースが必要になります。
  - 「GMSL2-USB3.0 変換キット」などで検索してみてください。
  - USBに変換して接続した場合には、評価目的の一時的なものと思ってください。
  - 本来のGMSL2のネットワーク性能は確保されません。
- ロボットの分野では、かなりの数のセンサ類・アクチェータ類とのインタフェースが必要になります。
  - 選択肢１：一つのボードという選択
    - その場合でも、System on Module (SOM)を使うことになるでしょう。
  - 選択肢２：高次の判定を行う側のボードとデバイス側（センサ類・アクチェータ類）に近いボードとに分けるという実装
    - この選択だと、デバイス側（センサ類・アクチェータ類）に近いボードを持つことで、インタフェースが足らなくなることを解消しやすくなります。
    - センサを置き換えるときの変更がデバイスに近い側だけのボードの改変に留めることができます。
    - デバイス側（センサ類・アクチェータ類）に近いボードの中で緊急性が高い処理を、短い時間で行うことが可能になります。
    

## dataの流れ
- 複数の台数のカメラを接続する時点で、ボードの選択は重要です。
- CPU-GPU間の画像データの転送が減らせることが大事です。
  - DMA転送が重要です。
- 利用するライブラリも、CPUの負荷が減らせるライブラリであることがのぞましい。
- 画像データを圧縮・展開するなどのオーバーヘッドを生じないこと。
- データを転送しても、データのタイムスタンプが付随していることが大事です。

## ボード選択に関する私見
- プロトタイプ段階では、十分な余力のあるボードを選択する。
- 実装する機能を選択する。
- 必須機能：
- 余力があるときの追加機能：
- ボードの選択は難しい。
あるIOで２種類の記載があっても、CPU側の制限によって同時に使えるのは１つだったりすることがある。
GMSL2のカメラを６系統受け入れられるボードであって、画像認識をも同時にこなせるボードはあまりない。
各自、調査することだ。

## 必用な機能がそろったら、使用するリソースの削減を考える。
- 画像の解像度の削減
- 機能の呼び出し回数の削減
- ターゲットデバイスでの実現可能性を明らかにする。

## 学習・評価用の環境からターゲットのボードへの移行
- 学習・評価環境からのdeploy方法を自動化させておく。
- 計算リソースに余力があるようにターゲットボードを選択する。

## ボードはいずれEOLになる
ベースとなる学習・評価用のデータ、学習・評価用のソフトウェアをメンテナンスし続けること。
将来、別のターゲットのボードがでてきたときに、変換してdeploy できるようにする。

[PyTorchをM1 MacBook のGPU(MPS)で動かす．実行時間の検証もしたよ](https://zenn.dev/hidetoshi/articles/20220731_pytorch-m1-macbook-gpu)
