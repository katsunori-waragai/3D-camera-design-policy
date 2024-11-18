# 開発に関わる細々したこと
## OpenCVは、どのような条件でビルドされていますか
- [ ] CUDAはどうなっていますか
- [ ] OpenCLはどうなっていますか
- [ ] FFMPEG
- [ ] GStreamer

```commandline
>>> import cv2
>>> print(cv2.getBuildInformation())
```

[NVIDIA Vision Programming Interface (VPI)入門 Python API編](https://qiita.com/dandelion1124/items/9b6a33b97f088f3fb205)

このようなインタフェースも考察に加えたい。

## python coding style
- 多くの引数を持つ場合には、キーワード引数で指定する。順序引数にすると間違いを生じる。
- 


## 画像の保存
- [0, 1]のfloat画像を保存するには、skimaie.io.imsave() を使おう。
- [0, 1]のfloat画像をuint8 にするには、skimage.img_as_ubyte()を使おう。

## Unicode文字列を画像に書き加えるには
- PIL.Image (pillow)を使おう。
  - https://pillow.readthedocs.io/en/stable/reference/ImageDraw.html
- PIL.Image はImageIOを使うと既にインストールされている。
- ImageIOはScikit-image を使うと既にインストールされている。

## Calltree で判明したこと
scikit-image はimage-io に依存
image-ioはPillow に依存。

エラーログの一部
```commandline
/usr/local/lib/python3.8/dist-packages/skimage/io/_io.py:143: in imsave
    return call_plugin('imsave', fname, arr, plugin=plugin, **plugin_args)
/usr/local/lib/python3.8/dist-packages/skimage/io/manage_plugins.py:205: in call_plugin
    return func(*args, **kwargs)
/usr/local/lib/python3.8/dist-packages/imageio/v3.py:147: in imwrite
    encoded = img_file.write(image, **kwargs)
/usr/local/lib/python3.8/dist-packages/imageio/plugins/pillow.py:444: in write
    pil_frame = Image.fromarray(frame, mode=mode)
```

## 勝手に壊れていく実装
- 以下のような場合に、移植した実装は勝手に壊れていく
### 利用停止になるダウンロード先
- Google Driveにおいてあったpre-trainedモデルが提供停止になっていた。
### モジュールの破壊的な仕様の変更

### OSのサポートのEOL
#### Ubuntuの場合
https://ubuntu.com/about/release-cycle
End of Standard Support ５年
End of Ubuntu Pro Support 10年

ボードやセンサ類での対応OSの問題にも直結する。

### その言語のそのバージョンのEOL

壊れるときは、１週間後だろうが壊れている。
- エラーメッセージの内容は、既存のドキュメンテーションには書かれていない。

### 勝手に壊れていくのを防止する。
- 自動化テストで、壊れていたらすぐに検出できるようにすること。
  - 同じバージョンのままでも、別のマシンで新規にinstallとテストを実行してみよう。
  - apt install や pip install でライブラリのバージョンが違ってくる。
#### 対策
- 必要なライブラリ・モデルファイルなどのデータを、自社のサーバに保存しておく。
- Dockerなどの仮想環境上で、システムを構築し、固定したバージョンを運用する。
- 運用先でのネットワークへの接続をさせない。
- 自動更新を無効化する。
  - 例: apt install, pip install をさせない。
  - [pipでもlockで依存パッケージバージョンを管理しよう](https://zenn.dev/yhay81/articles/yhay81-202102-piplock)
