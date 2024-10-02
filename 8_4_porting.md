# 移植
### TensorRTへの移植
Cuda　デバイスへの移植の際に実行速度を早めるためには、オリジナルのモデルをTensorRTのモデルに変換する。
このことによって、実行速度が改善する。

https://github.com/NVIDIA-AI-IOT/torch2trt

### OpenVinoへの移植
Intelの提供しているOpenVinoの枠組みは、複数の種類のIntelデバイスへのdeployが可能である。 
対象: インテル製の CPU や GPU、VPU、FPGA 
OpenVinoのすばらしい点は、GPUなどをもっていないマルチコアのIntel CPUでも実行できるという点にある。

mediapipeのopenvinoへの実装例では、手のランドマークの検出が、ノートパソコンのCPUでも応答性よく動作する点にある。


https://docs.openvino.ai/2024/openvino-workflow/model-preparation/convert-model-to-ir.html

### mediapipeのopenvinoへの実装例
https://docs.openvino.ai/2024/ovms_docs_mediapipe.htmlo
