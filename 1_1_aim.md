# この構想の目的
- 3Dカメラの利用と、画像認識・３D再構築・機械学習などを扱いやすくする設計方針を示すこと
- アルゴリズムの置き換えを簡単にしていくこと
- ３Dカメラそれ自体についてのインタフェースについて、SDKごとの差異をわかりやすくする。
- 可能ならば、それらが類似のインタフェースによってデータの取得できること。

## 参考事例
scikit-learn

scikit-learnでは、学習アルゴリズムが異なってもfit(), predict()
という共通の名前をメソッドに使っていることで、
アルゴリズムを差し替えを簡単にしている。

```.py:
from sklearn import svm
X = [[0, 0], [1, 1]]
y = [0, 1]
clf = svm.SVC()
clf.fit(X, y)
clf.predict([[2., 2.]])
```


RandomForestClassifierの場合

```.py:
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import make_classification
X, y = make_classification(n_samples=1000, n_features=4,
                           n_informative=2, n_redundant=0,
                           random_state=0, shuffle=False)
clf = RandomForestClassifier(max_depth=2, random_state=0)
clf.fit(X, y)
clf.predict(X)
```

どちらの実装の利用においても
```commandline
clf.fit(X, y)
clf.predict(X)
```

部分は共通である。
このため、分類器　Classifierの実装が置換えやすいインタフェースになっている。

混同行列(confusion marix)を計算して、どういう誤分類を生じているのかをテーブルにまとめるのは
共通の方法で対処できる。

[sklearn.metrics.confusion_matrix(y_true, y_pred, *, labels=None, sample_weight=None, normalize=None)]
(https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html#sklearn.metrics.confusion_matrix)

```.py:
>>> from sklearn.metrics import confusion_matrix
>>> y_true = [2, 0, 2, 2, 0, 1]
>>> y_pred = [0, 0, 2, 2, 0, 2]
>>> confusion_matrix(y_true, y_pred)
array([[2, 0, 0],
       [0, 0, 1],
       [1, 0, 2]])
```

このような共通のインタフェース設計が、機械学習のライブラリを利用しやすくした。


## この宣言がめざすこと
- タスクへの入出力が同じアルゴリズムであれば、アルゴリズムの種類が変わっても、同一のインタフェース設計でコードを書ける。
- 例：単一フレームでのステレオ計測のアルゴリズムの実装であれば、利用するアルゴリズムの種類や実装のフレームワークが違っても、 同じインタフェースで利用可能な状況を目指すこと。
- 依存するCPUボード・アクセラレータが変わっても、有用なアルゴリズムが移植可能な状況を実現しやすくすること。
  - 3Dカメラを差し替えても、可能な範囲で画像認識・３D計測が同一のインタフェースで組合せ可能にしていくこと
    - 例：物体検出の結果を３Dカメラからの３Dデータと関連付けるを簡単にしていくこと。
- 3Dカメラの種類の違いによる比較をしやすくしていくこと
  - 現状では、それぞれのSDKでの表示ツールでの表示に頼っていることが多い。
  - そのため、違いが表示ツールによるものなのか、特性の差によるものかがわかりにくい
  - 共通の表示ツールを使えるようになれば、特性の差を比較しやすくなる。
- ある3DカメラのSDKのサンプルプログラムが有用でしかもOSSのライセンスであることがある。
  - そのよいアイディアを、別のカメラでも使いたい。

## 私たちにできること
- 利用している３Dカメラの違いを超えた３Dカメラ利用者のコミュニティの創出
- ある３Dカメラにある実装を、他の３Dカメラへの移植の可能性の共有
- 利用するOS、CPUボード、GPU、アクセラレータの違いを超えての情報の共有化
- 視覚言語モデルなどの進展を取り入れやすくすること
