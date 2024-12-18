# アルゴリズムの精度の評価

## そのタスクの標準的なデータ・セット
PapersWithCode　や論文自体から探すと良い。
#### 例：ステレオマッチング
https://paperswithcode.com/task/stereo-matching-1/latest#datasets
###### 正解データ作成のやっかいさ
- ステレオマッチングの場合においては、CG画像を学習と評価に用いることが増えている。
- それは、実環境のデータに対して真値の深度を与えることがとてつもなく難しいからだ。
## 業務上の開発においては、データセット構築の方がアルゴリズム改変よりも優先する。
#### 既存データ・セットを用いて、利用しようとするアルゴリズムでの評価環境を作る。
- 推論の精度を評価できる指標を見つけ出す。
  - そのタスクの分野の論文には、どのように評価したのかを書いてあるはず。
  - scikit-learnには、様々なタスクで使われている様々なscoreが用意されている。
- 評価の自動化環境を作る
  - チームの同僚が簡単にトレースできるように作ること
## 自前データセットの構築
自分たちが評価するタスクを明確にする。
そのタスクの分野で、用いられているデータセットの構造を調べる。
そのデータ・セットを模倣するデータセットを自分たちの目的のために作る。

#### クラウドサービスを利用したデータセットの保存
- データレイクとデータウェアハウス
- あらっぽい説明
  - データレイク：取得しただけで、利用のための準備が整っていないデータも含む
  - データウェアハウス：活用するための前加工が終わっているデータ

### Data Version Control
方針：タスクのアルゴリズムの性能を評価するデータセットは、データセットのバージョン管理をできるツールを利用する。
方針：データのクラウドストレージは、クラウドサービスを複数の選択肢から選べるものを用いる。
当面の手法：　Data Version Control をデータ管理のツールとして用いる。

###### 注意：データ喪失の危険
- 機械学習で利用するデータは、間違った操作で簡単に失います。
- 特に操作に不慣れな場合は、間違えた操作をしがちです。
- 対策案：
  - 失うことを許されない本番データの前に、別のデータを使って練習をしましょう。
  - 一人だけが知っている状況ではなく、複数メンバーが理解している状況を作りましょう。
  - クラウドサービスの利用者の管理者権限を、引き継ぐこと

###### 注意：個人情報の秘匿
- 機械学習の分野では、個人情報の秘匿が重要になります。
- 以下のような本を参考にしてください。
- [データ匿名化手法 ―ヘルスデータ事例に学ぶ個人情報保護](https://www.oreilly.co.jp/books/9784873117249/)


### 評価の自動化
- アルゴリズムをバージョン管理し、データもバージョン管理したうえで、評価の自動化を行います。
- 評価結果を誰がどの程度の頻度で見ますか？
- 学習済みモデルの運用は、どのような予定ですか？
- これらによって、評価の自動化の選択肢があります。
- MLOpsと呼ばれる分野のサービスは、頻繁に変わっているので、最新の技術は各自調査してください。
##### 選択肢: Google Compute Engineの利用
- Google のサービスとして仮想の計算機を用意して、その中で任意の処理を通常のLinuxマシンのように利用します。
- ハイエンドのGPUや、十分に潤沢なメモリでの学習・評価が可能になります。
##### 選択肢: Google Colaboratoryの利用
- Jupyter notebook 類似の環境です。
- 結果をmatplotlibを使ってグラフ化したり、pandas を使ってデータの集約をしたりするのが得意です。

##### 機械学習エンジニアに、DevOps エンジニアからの手助けを
- 機械学習・画像認識のエンジニアは、クラウドサービスの構築のプロではありません。
- 社内で彼らの手助けを得られるようにしてください。
- [DVC で利用可能なクラウドストレージサービス](https://dvc.org/doc/user-guide/data-management/remote-storage)
```commandline
Supported storage types
Cloud providers
Amazon S3 (AWS) and S3-compatible e.g. MinIO
Microsoft Azure Blob Storage
Google Cloud Storage (GCP)
Google Drive
Aliyun OSS
```


##### 画像系のエンジニアには、データベースエンジニアからの手助けを
- 画像系のエンジニアは、データベースのプロではありません。
- python使いの場合には[sqlite3](https://docs.python.org/ja/3/library/sqlite3.html) を使って簡単なsqlデータベースを使う場合もあります。
- 本格的なデータベースを構築する必要があれば、その道のプロに任せることです。
  - データベースサーバーの運用・バックアップ・セキュリティ対策

## 3D計測部分についてのテスト
- 3D計測の部分のロジックを再現できる場合：
- 場合１：left画像, right画像が保存されているので、同一画像から再度ステレオ計算ができる。
- 場合２：その３DカメラのSDKに固有のデータ形式で画像を含むセンサデータを保存し、それを再生することで、あたかも今カメラが撮影しているかのように振る舞わせる。
  - 例：ZED SDK(StereoLabs)はそのようなことが可能です。
  - この場合には、同じ入力データに対する動作モードによる３Dの計測結果の比較が可能です。
- 場合３：３Dカメラでの計測結果を信じるしかない場合
  - ToFカメラの場合には、ToFカメラが出力した距離画像は、そうであると受け入れるしかない。
  - それ以前の信号を元に距離画像とする部分のロジックはブラックボックスである。

## 予算管理者へのお願い
- 3Dカメラの価格は１０年前に比べて飛躍的安くなっています。
- あれこれ悩んでいる暇があったら、数万円程度のものなら、さっさと購入するのがいい。
- 製品のwebページやユーザーの評価事例をどれだけ読んでも、あなたのユースケースでの性能はわからない。
