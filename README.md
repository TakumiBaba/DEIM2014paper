1/10 投稿
人をプログラム要素として使える仕組み
人やらせたい手順と、自分にやらせる手順
概要
 人を万能に利用可能なプログラム要素として組み込むことのできるシステムを提案する
 コンピュータの処理を記述するプログラムと人の処理を記述するマニュアルがあるが、同じ処理を記述するものなのに、二つはまったくの別物となっている
  同じ手順を記述するものならば、プログラム内に人の行動が記述できると良い
   双方の処理結果を活かす
   マニュアルを実行可能にする
 人オブジェクトを宣言可能にするライブラリと、人オブジェクトに送られた命令を受信し、その結果をプログラムに返すことのできるクライアントアプリケーションを組み合わせることで実現させる
はじめに
 背景
  コンピュータに対する命令の記述
   コンピュータに実行させたい計算について記述する
   プログラム
  人に対する命令の記述
   どう行動するのか、といったことがまとめられる
   マニュアルやレシピなどの手順書
    プログラムのようなもの
    繰り返し、ifなどの考え
   人に対する命令の記述もプログラムである
  プログラム上で人/コンピュータ、両方への命令を記述する
   コンピュータのプログラムと人のプログラムを融合させる
    人が得意なことや、人にしかできないことは人にやらせる
    コンピュータが得意なことはコンピュータにやらせる
   共通化することで、コンピュータと人の双方の良いところを活かすことができる
   プログラムとして記述することで、レシピやマニュアルは実行可能なものとなる
    正確な時間に実行、何かしらの外部イベントを元に実行、といったことが可能に
  世の中のあらゆることをプログラムとして記述する
   コンピュータへの命令系統
    従来の手法通り
   実世界を操作するための命令系統
    センサ・アクチュエータである程度できるようになっている
   人間への命令系統
    人間の行動そのものを記述するためのものはなかった
     人を計算機の代替として扱うものは存在するが。
    身近な人であったり、自分を動かすためのプログラムを書くことは考慮されてこなかった
  これに適した仕組みがない
   コンピュータ・実世界・人のすべてをプログラムするための仕組み
 プログラムに人を組み込め、命令できる仕組みを提案する
  プログラムに人を組み込めれば、プログラム上でコンピュータと人両方への処理を記述可能になる
   もちろん、実世界を操作することも可能
  周りの実世界環境の全てをプログラミングするための第一歩として
  試作した
提案
 人をプログラム要素として使える仕組み
  人の行動をプログラム上で記述可能にする
   プログラム上で人を表現可能にし、それに対し命令を送る
  人をプログラム上でオブジェクトとして表現可能にする
   通常のオブジェクトを利用するのと同じように
    オブジェクトに対し命令すれば、返り値が得られる
    プロパティが変更されたりする
  そのためには以下のような要素が必要だ
   1. プログラム上で人をオブジェクトとして表現可能
   2. 命令を処理、その結果をプログラムに返す
 以下の手法で実現する
 人をオブジェクトとして表現でき、命令を送れるライブラリ + 命令を受け取り、その結果を返すことのできるクライアントアプリケーション
  BabaScript
   既存のプログラムに組み込めるように
   node.js ライブラリとして実装
  BabaScript Client
   Web / iOS / andoird クライアントアプリケーションとして実装
 流れ
  プログラムで人への命令構文を書く
  ネットワークを介して、適切なBabaScript Clientに命令が配信される
  BabaScript Client は、命令をユーザに通知する
  人が命令を処理する
  人は処理結果をBabaScript Clientに入力する
  結果をネットワークを介して、プログラムに返す
  プログラムは、返ってきた値を元に指定された続きの処理を実行する
 特徴
  通常のオブジェクトへの命令と同じような手法で、人へとアクセス可能
   返り値を得られたり、プロパティを変更可能
   普通に非同期Callbackのプログラムを書くのと同じようにできる
  特定の人(たち)をプログラムに組み込める
   クラウドソーシングなどとは違い、特定の人に対して命令配信が可能
  あらゆるところで命令に対して返事ができる
   人は常にパソコンの前にいるわけではない
   実世界の事物に干渉するときなどは、離れなくてはいけないし、どこでもできるようにすべき
   クライアントアプリケーションをスマートフォン上で実装
  本システムを拡張することで、誰でも簡単に自分用の人力プログラム・クライアントアプリが作れる
仕組み
 BabaScript
  人間のへの命令配信手法
   baba = new BabaScript("baba")
   baba.書類を片付ける {}, ()->
    hogefuga()
   上記のようなコードで、 "書類を片付ける" という命令を baba という id を持つ人間(たち)に配信することができる
   baba オブジェクトに実装されていない全てのメソッドが、人への命令として配信される
  コールバック関数
   人力メソッドの第2引数は、コールバック関数
   人から値が返ってくると、この関数が実行される
  format
   BabaScript Client 及び、の応用実装が利用可能な format を指定可能
   BabaScript Client では、 Boolean ,String, Int, List が利用可能
   人間への命令構文の第一引数の部分で指定可能。
    {format: "boolean"}
    {format: "string"}
  timeout
   指定する時間が経過したら、命令をキャンセルして、その時点で得られているデータをコールバック関数の引数に与え、実行する
   {timeout: sec}
 BabaScript Client
  通信用のクライアント
   next()
   etc
  既存の仕組みに追加して利用可能
   javascript
    client = new Client("baba")
    client.on("get_task", function(){})
   js が動かせる環境ならあらゆるデバイスがBabaScript Clientとなる
  クライアント側で、IDを指定する
   IDごとに命令が配信される
   命令を受け取ると、命令に応じてインタフェースを変化させ、ユーザの処理を促す
  Boolean, String, Int, List のViewを持っており、optionに含まれるformat の中身によって切り替える
   formatに応じてViewを切り替えることで、欲しい情報を得る
  client.on "get_task", ()->
   クライアントオブジェクトに対し、get_task というイベントで関数を登録しておくと、命令を受け取った時にこの関数が実行される
 サンプル
  BabaScriptのプログラム
  Clientのインタフェース
応用例
 人の仕事や役割をプログラム化する
  料理の一連の流れをプログラム化するとか
 柔軟な実世界プログラミング
  普通のセンサ・アクチュエータでは不可能なセンシング・アクチュエーションを行う
  「その場の空気」の数値化・文字列化
  より複雑な動きを伴うアクチュエー
  センサ・アクチュエータの代替機能
  その場に機械がなければ、人にセンサ・アクチュエータのような挙動をさせる
 プログラムのエラー処理
  人をエラー処理に組み込む
  人の判断を待って、エラー処理を実行する
関連研究
 人を計算資源として利用する手法はヒューマンコンピュテーションと呼ばれ、その利用手法としてのクラウドソーシングと同様に多くの既存研究が存在する
  Human Computation
   ヒューマンコンピュテーションは，計算機が処理できないタスクを人力を利用して解決すること
   そういった概念を提唱している
  Mechanical Turk
   米amazonが運営しているクラウドソーシングプラットフォーム
  AUTOMAN
   通常のプログラム言語の中で、デジタルのコンピューティングとヒューマンコンピュテーションを統合する
   タスクをインスタンス化する、自動スケジューリングなど
   プログラマにとってクラウドソーシングをより容易に扱いやすくしようとしている
  CrowdForge
   MapReduce like なツールキット
   タスクを適切に分割(Map)し、クラウドソーシングで分散的に人に解かせる。そして、集める(reduce)
  Jabberwocky
   プログラマがクラウドソーシングのためのプログラム(タスク)を簡単に作れる・再利用できるようにした
   クラウドソーシングのためのコミュニティを簡単に作れる
   ワーカー個人のスペックを配慮可能
  Turkit
   turkitは関連で挙げなくても良いかな？
  CrowdDB
   機械だけでは答えられないようなクエリに対し、クラウドソーシングを使うことで返答するためのSQLライクな言語
  CyLog/Crowd4U
   CyLog は、Datalog-likeのプログラミング言語
   ヒューマンコンピュテーションのための仕組みがクラスに組み込まれてる
   人をデータソースとして、プログラム中で使える
   Crowd4Uは、CyLogを使ったもので、クラウドソーシングを利用したアプリケーションを作りやすくするツール
  これらは、計算資源として人を利用しようというもの
   人にコンピュータの代わりをさせる
   人をデータソースとして利用しよおうというもの
  本研究は、プログラム上で人がすべき処理を記述可能にするものである
   クラウドソーシングが目的としている計算機機能の代替も機能の一部分
   主な目的は、人の行動をプログラムに組み込むこと
   コンピュータの代替も可能だが、主な目的は人の行動をプログラムに組み込むこと
 Human as Sensor系
  Using Stranger as Sensors:  Temporal and Geo-sensitive Question Answering  via Social Media
 業務手順の記述
  http://www.questetra.com/ja/
おわりに
 今後
  実行保障性
  割り込みとかどうするの
謝辞
 [[未踏]]ですよ、と言わなきゃいけないらしい
Infrastructure as Code みたいな言葉が欲しい
[[論文]] [[BABAScript]]