# ボードの選択
PCやMacを使うのでなければ、ボードの選択が必要になる。
(PCの場合にはGPUのボードということもある。)
MacでもPyTorchが利用できるらしい。

### NVIDIA Jetson AGX Orin
シングルボードコンピュータとして、NVIDIA Jetson AGX Orinを使うことができます。

SOM　Jetson Orin NX, Jeston Orin Nano

大半の機械学習のフレームワークが動作する。

### Raspberry Pi
raspberry pi 5 

##### raspberry pi 5+「Raspberry Pi AI Kit」
[ラズパイ5にAIキットが登場、処理性能は13TOPS](https://monoist.itmedia.co.jp/mn/articles/2406/05/news086.html)

### Qualcomm
[Qualcomm Neural Processing SDK for AI](https://www.qualcomm.com/developer/software/neural-processing-sdk-for-ai)

### SONY SPRESENSE
[SPRESENSE](https://www.sony-semicon.com/ja/products/spresense/index.html)
[Neural Network Console](https://dl.sony.com/ja/)
【Neural Network Consoleクラウド版サービス終了のお知らせ】
Neural Network Console クラウド版は2024年12月25日をもってサービスを終了させて頂きます。長い間ご愛顧頂きありがとうございました。
なお有償の法人向けプランをご利用頂いているお客様向けには別途終了時期のご連絡をさせて頂きます。
またNeural Network Console Windows版は提供を継続いたしますので引き続きご活用ください。

### Renesas　kakip
[kakip](https://www.kakip.ai/)

### Hailo
[Hailo-8 AI Accelerator](https://hailo.ai/ja/products/ai-accelerators/hailo-8-ai-accelerator/)

[Hailo-8 Edge AI Accelerator Deployment Guide](https://tlab.hongo.wide.ad.jp/2024/03/04/hailo-8-edge-ai-accelerator-deployment-guide/)

HailoModelZoo/docs/GETTING_STARTED.rst
https://github.com/hailo-ai/hailo_model_zoo/blob/master/docs/GETTING_STARTED.rst

### Coral USB Accelerator
[Coral USB Accelerator](https://coral.ai/products/accelerator/)

### VPU
[Movidius™ ビジョン・プロセシング・ユニット (VPU)](https://www.intel.co.jp/content/www/jp/ja/products/details/processors/movidius-vpu.html)
Movidius Neural Compute Stick (NCS),


シングルボードコンピュータとはならなもの

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
- 開発のしやすさ
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
