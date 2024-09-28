# この構想の目的
- 3Dカメラの利用と、画像認識・３D再構築・機械学習などを扱いやすくする設計方針を示すこと
- アルゴリズムの置き換えを簡単にしていくこと

## 参考事例
scikit-learn

scikit-learnでは、学習アルゴリズムが異なってもfit(), predict()
という共通の名前をメソッドに使っていることで、
アルゴリズムを差し替えを簡単にしている。

```
from sklearn import svm
X = [[0, 0], [1, 1]]
y = [0, 1]
clf = svm.SVC()
clf.fit(X, y)
clf.predict([[2., 2.]])
```
