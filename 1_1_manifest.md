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
- 3D分野の初心者にとってよい初期値を与えること
  - 3D分野の初心者が利用するだろうステレオ計測の実装はOpenCVのものだろう。
  - しかし、OpenCVのステレオ計測は、2020年代の今となってはあまりにも古い実装になっている。

#### 3Dカメラと画像認識との統合は、新しい産業の基盤となる
- 機械学習が、従来はファイルに変換済みのデータに対するもので、身体性をあまり持たないものが中心だった。
- しかし、大規模言語モデルの成功は、それらをカメラとアーム・ハンドをもつ身体性のある知能に変えつつある。
- 視覚言語モデルに基づく汎用性が高い学習は、アーム・ハンドと結びついて、新たな時代を切り開こうとしている。
- 視覚言語モデルの成功は、それが７軸アームとグリッパーの組合せであっても、様々な分野で産業構造を変えると予想されている。
- その中で、3Dカメラと認識・計測技術が標準化されないものになってしまうと、使い勝手の悪さのために、なかったと同じものになってしまう。
- 使い勝手を向上させることが、新しい基盤になるうえで必要だ。

##### 今回のAIの進展が期待はずれとなりにくい理由
- かつて、AIへの期待が広がった時期が会って、期待されるほどには結果を出せずに、AIの冬の時期があった。
- 今回のすぐに冬の時代をむかえるだろうと思っている人もいるかもしれない。
- しかし、私は、今回の進展は一時的なブームではなく、継続的な進展になると確認している。
- 理由：
  - 部分的に役に立つタスクを様々にこなしていること
  - 制約の少ない条件でのタスクをこなしていること
    - 例：対象物を限定しない物体検出
    - 例：画像と言語の連携が進んでいること
    - 例：機械学習結果を利用して機械学習を改善するループが回りだしていること
  - カメラと自律マシンとの間で、外界についての「理解」を可能にしてきていること
  - 今回のAIの開発では、コミュニティの規模が大きくなっていることによる加速が生じていること

##### 国内の動向への危惧
- 今回の技術の進展に対する、産業界の対応が乏しいこと
- 3Dカメラそれ自体、大規模言語モデルの画像認識AIへの影響、３Dとの組合せによる産業への影響
- 技術の進展をトレースするのさえ追いついていない。
- 

## 公開されることで開発が加速される。
#### コミュニティの規模が開発を加速させる。
- 「こういうのほしいよ」、「実装してみたよ」の発生するイベントの数は、コミュニティの規模Nの2乗に比例する。
- 「実装してみたよ」、「使ってみたよ」の発生するイベントの数は、コミュニティの規模Nの2乗に比例する。
- 「製品を販売します」、「購入します」の発生するイベントの数は、コミュニティの規模Nの2乗に比例する。
こういう状況において、英語圏・中国語圏のコミュニティにくらべ、日本語のコミュニティの規模は小さい。
それなのに、アイディアや、秘匿するまでもない実装を秘匿することは、圧倒的な遅れを引き起こす。
コミュニティを作る努力は、論文の調査の協力や、コミュニティのメンバー間の努力によってなされている。
3Dカメラと画像認識・視覚言語モデルなどのコミュニティが活発になることは次の効果をもたらすと考えている。

#### 僕たちの実現しようとする未来は、自部署だけじゃ作れない
- 僕たちが山ほど書いてきた実装のほとんどは、その時点では会社の売上に貢献したかもしれない。
- しかし、それらのコードは、その次の未来を築くことなく、古びて失われていった。
  - GitHub以前では、コードを確実に次の世代のメンバーに受け渡す方法がなかった。
  - 担当した人が職場からされば、その存在さえ忘れさられる。
  - Not Invented Here の価値観は、評価してみるよりも先に、自分で書き出すのをしがちだろう。
- 僕たちの作ろうとする未来において、３Ｄ計測と画像認識と視覚言語モデルの融合は、どこかの誰かによって支配されることを望まない。
  - 協力して未来を築かなければ、どこかの誰かに支配されて終わってしまう。
  - 支配された状況では全ての富は、その技術を支配する人たちに握られてしまう。
  - 支配された状況では、さらなる改良を推進できない。
- 自部署だけのノウハウとして秘匿する部分があっていい。
- それ以外の秘匿する価値のない部分は共有して、APIをそろえていこう。
- 秘匿するコードもAPI設計を共通にしておけば、商業的にサポートしているアルゴリズムの力を見せつけることが簡単だ。
- くり返し言う。僕たちの実現しようとする未来は、自部署だけじゃ作れない。

#### 効果
- 初心者が、その分野の最新の状況に入りやすい。
- 3Dカメラの選択、ボードの選択、ライブラリの選択などについての情報を得やすい。
- それぞれの方式が、どういう条件では効果的であり、どういう条件では使い物にならなくなるのかがわかりやすい。
- SOTAに準じるアルゴリズムを早い段階で利用できる。
- どういうAPI設計をすれば、ライブラリとして共有しやすいのかがわかる。



## 私たちにできること
- 利用している３Dカメラの違いを超えた３Dカメラ利用者のコミュニティの創出
- ある３Dカメラにある実装を、他の３Dカメラへの移植の可能性の共有
- 利用するOS、CPUボード、GPU、アクセラレータの違いを超えての情報の共有化
- 視覚言語モデルなどの進展を取り入れやすくすること
