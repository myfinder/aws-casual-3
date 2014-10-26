# 僕らのAWS移行記
AWS Casual Talks #3 @ Cookpad

id:myfinder

___

# 自己紹介
- id:myfinder
 - まいんだー
- エム・ティー・バーン株式会社
 - サーバサイド / Android

---

# 経緯
## 星野さんに反応したら
## 星さんから連絡が来た

---

# 先にお知らせ

---

# MySQL Casual Talks vol.7
- 12/12(金)開催
- 参加登録 -> いまから
- トーク/LTネタがある方はお気軽に^^

---

# 今日話すこと
- どオンプレで作られたサービスをAWSに移設しました
- 移設するにあたって、大きく運用環境を変えました
- AWS x Mackerel がとても捗ります

---

# 移行前の環境
オンプレ+S3/CloudFront

![servers](images/servers.png) ![s3](images/s3.png) ![cf](images/cf.png)

---

- 海外展開で初めてEC2/ELBを使い始めた程度
 - AWS Summit 2013(http://aws.amazon.com/jp/solutions/case-studies/freakout/)

---

# 移行前のシステム環境
- Single Segment
- データセンター提供のマネージドDNS
- 箱物ロードバランサ / 大手メーカー製サーバ
- CentOS 6.x / cobbler / puppet
- Hive / MapReduce
- MySQL / Redis おじさんが用意/運用
- Nagios / CloudForecast
- 足元のディスクに吐いたログをでっかいサーバに収集して集計というWeb1.0

---

# 懸案
## "調達"と"実装スピード"
## インフラおじさんへの依存

---

# 移行にあたって
- Single Segment -> Multi-AZ
- マネージドDNS -> Route53
- 箱物LB -> Elastic LoadBalancing
- CentOS -> Ubuntu
- cobbler -> cloud-init
- puppet -> ansible
- Hive -> BigQuery
- MySQL -> RDS(MySQL)
- Redis -> ElastiCache(Redis)
- Docker を一部でつかい始める
- Nagios+CloudForecast -> Mackerel

---

## アプリケーションサーバのつくり
- 足元のストレージに吐いたログを収集して集計していた
- アプリケーションからfluentdへログを投げる形に

---

## Single Segment -> Multi-AZ
- オンプレ脳でネットワークを作ったらやっちまった事例
 - http://www.slideshare.net/Yuryu/aws-casual-talks-1-vpc
 - http://aws.amazon.com/jp/solutions/case-studies/freakout/ <- これがそのやっちまった事例

---

## マネージドDNS -> Route53
- 履歴機能とかはあったけど余りプログラマティックではなかった
- Roadworkerで履歴管理できるのでさくっと引越

---

## 箱物LB -> Elastic LoadBalancing
- ハードウェアロードバランサにはとても救われた
- 一方でハードウェアロードバランサおじさん業が必要になる
- 最初からキャパシティを見切れない環境では新規に買うのもためらわれる

---

## CentOS -> Ubuntu
- 事例の時は独自の AMI を作っていた
 - update への追随がダルくなるのでやめたかった
 - AWS でまともな CentOS を使おうと思うと Rightscale の AMI を魔改造する羽目になる
- 標準提供のものと付き合う方が他のメンバーにも理解が早い
- 独自の yum リポジトリとか作っていたけどそういうのを捨てた
 - いうほど独自リポジトリ必要ですか？

---

## cobbler -> cloud-init
- ハードウェアの種類が多いと cobbler の snippet がカオス化したりする
 - ex. ethx で良かったはずの記述が DELL のサーバ買った瞬間 enx の対応を迫られる
 - ex. Hadoop のために 2U のサーバみたいなのを買うとどでかいパーティションをほげほげする対応に迫られる
- EC2 の場合素直にセットアップしたいことをシェルスクリプトで書いておけば良い
- そもそもビッグデータは後述の BigQuery に移行することで自分で持たないから関係なくなる :p

---

## puppet -> Ansible
- chef-soloも検討したけどAWS移行してセットアップで複雑なことをやる必要がなくなった
 - yaml を書くだけでいい Ansible の方がメンバーの学習が早いので採用
 - if とか必要ない(必要な状況になったらおとなしく role を分けるべき)
- Ansible の Dynamic Inventory 機能も大きかった(Mackerel連携)

---

## Hive -> BigQuery
- ログを貯めて集計する場所を自分で作らないでも良い時代になった
- Redshift も比較したけど現時点では BigQuery が勝ってる、AWS 頑張ってほしい

---

## MySQL -> RDS(MySQL)
- 事例のタイミングではオペレーションが変わるのを避けたかった
- オンプレ脳で作ったネットワーク&オンプレとの連携のせいで生かせなかった反省を踏まえ今回は採用
- MySQL Casualもよろしくね!!

---

## Redis -> ElastiCache(Redis)
- バックアップや Failover のことを考えるときに自前で運用するのをやめた
- Multi-AZ
- click データの長期保持が必須だったがこれで解決
- auto failover サポートでさらに安心

---

## Dockerとつきあいはじめる
- fluentd の collector を Docker で動かしています
- Deploy は cap で docker pull -> docker stop -> docker run
- graceful restart が必要なコンポーネントにはまだ課題がある

---

## Nagios + CloudForecast -> Mackerel + Slack
- オンプレではサーバリストの管理をどうするこうするという問題がついて回ってきていた
- クラウドに移行するにあたって、個別のサーバに命名することをやめた
- 個別のサーバにログインするオペレーションが大幅になくなった

---

# AWS x Mackerel
- 軽いデモ

---

# おわりに

- どオンプレで作られたサービスをAWSに移設しました
- 移設するにあたって、大きく運用環境を変えました
- AWS x Mackerel がとても捗ります

---

# おわり
