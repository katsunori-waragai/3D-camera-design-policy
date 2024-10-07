# ボードの選択
PCやMacを使うのでなければ、ボードの選択が必要になる。
MacでもPyTorchが利用できるらしい。

### NVIDIA Jetson AGX Orin
シングルボードコンピュータとして、NVIDIA Jetson AGX Orinを使うことができます。

SOM　Jetson Orin NX, Jeston Orin Nano

### Raspberry Pi
raspberry pi 5 

##### raspberry pi 5+「Raspberry Pi AI Kit」
[ラズパイ5にAIキットが登場、処理性能は13TOPS](https://monoist.itmedia.co.jp/mn/articles/2406/05/news086.html)

### Qualcomm
[Qualcomm Neural Processing SDK for AI](https://www.qualcomm.com/developer/software/neural-processing-sdk-for-ai)

### SONY
[SPRESENSE](https://www.sony-semicon.com/ja/products/spresense/index.html)
[Neural Network Console](https://dl.sony.com/ja/)
【Neural Network Consoleクラウド版サービス終了のお知らせ】
Neural Network Console クラウド版は2024年12月25日をもってサービスを終了させて頂きます。長い間ご愛顧頂きありがとうございました。
なお有償の法人向けプランをご利用頂いているお客様向けには別途終了時期のご連絡をさせて頂きます。
またNeural Network Console Windows版は提供を継続いたしますので引き続きご活用ください。

### Renesas
[kakip](https://www.kakip.ai/)

### Hailo
[Hailo-8 AI Accelerator](https://hailo.ai/ja/products/ai-accelerators/hailo-8-ai-accelerator/)

### Coral USB Accelerator
[Coral USB Accelerator](https://coral.ai/products/accelerator/)

### VPU
[Movidius™ ビジョン・プロセシング・ユニット (VPU)](https://www.intel.co.jp/content/www/jp/ja/products/details/processors/movidius-vpu.html)


## ボード選択のポイント
- CPU, GPUなどのアクセラレータ, OS
  - 演算性能が足りているか
  - 起動しっぱなしになるプロセス・スレッドの数に対して、コア数が十分か
- インタフェース
  - カメラインタフェース
  - USBインタフェースの数
  - ネットワーク性能
  - wifi の対応の有無
- 開発のしやすさ
  - HDMIポート, DisplayPort
  - 開発時のメモリの潤沢さ
  - 開発時のディスク領域の潤沢さ

## カスタムボード
- カメラの数を増やす、カメラインタフェースを追加するなどの状況では、開発キットにあるボードコンピュータでは適さなくなります。
- そのボードのSystem on Module (SOM) を利用しつつ、それと組み合わせるカスタムボードが必要になります。
- これらは、画像認識屋・機械学習屋の領域を超える範囲の作業です。
- しかるべき人に対応してもらいましょう。

## dataの流れ
- 複数の台数のカメラを接続する時点で、ボードの選択は重要です。
- CPU-GPU間の画像データの転送が減らせることが大事です。
  - DMA転送が重要です。
- 利用するライブラリも、CPUの負荷が減らせるライブラリであることがのぞましい。
- 画像データを圧縮・展開するなどのオーバーヘッドを生じないこと。
- データを転送しても、データのタイムスタンプが付随していることが大事です。
