## 参考事例: scikit-learn

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

共通のインタフェースの利点は、以下のようなコードで、4種類のアルゴリズムに対して、
fit(), predict()が共通なことが効いている。

```.py:
# Fit estimators
ESTIMATORS = {
    "Extra trees": ExtraTreesRegressor(
        n_estimators=10, max_features=32, random_state=0
    ),
    "K-nn": KNeighborsRegressor(),
    "Linear regression": LinearRegression(),
    "Ridge": RidgeCV(),
}

y_test_predict = dict()
for name, estimator in ESTIMATORS.items():
    estimator.fit(X_train, y_train)
    y_test_predict[name] = estimator.predict(X_test)
```

[引用元のスクリプト](https://scikit-learn.org/stable/auto_examples/miscellaneous/plot_multioutput_face_completion.html#sphx-glr-auto-examples-miscellaneous-plot-multioutput-face-completion-py)

![](https://scikit-learn.org/stable/_images/sphx_glr_plot_multioutput_face_completion_001.png)
スクリプトの実行結果
このデモスクリプトは、顔の上半分を与えたら、顔の下半分を予測させるモデルの学習後の予測の結果だ。
顔の特徴は、上半分と下半分で無関係なわけがない。顔が丸顔だったら、上半分と下半分で共通だろし、年齢や性別の影響も上半分と下半分で共通に現れると期待される。
だから、上半分の顔画像を与えたときに、下半分の顔を予想させようという取り組みが可能になる。
使用した手法によっては、それらしい結果を与えている。
（生成AIを用いた状況では、これらよりも良い推論を得られるだろう。）

このようにアルゴリズムが異なっても同一のインタフェースを持つことの利点は大きい。
