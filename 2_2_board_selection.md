# ボードの選択
PCやMacを使うのでなければ、ボードの選択が必要になる。
MacでもPyTorchが利用できるらしい。

### NVIDIA Jetson AGX Orin

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
