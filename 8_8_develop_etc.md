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

## ユニコード文字列を画像に書き加えるには
- PIL.Image (pillow)を使おう。
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
