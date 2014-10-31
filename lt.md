# Redshift
# vs
# BigQuery

AWS Casual Talks #3 @ Cookpad

id:myfinder

___

# 先にお知らせ

___

# MySQL Casual Talks vol.7
- <span style="color: red">12/12(金)</span>開催
- Zussar力が低くて応募できないので後でどうにかします
- トーク/LTネタがある方はお気軽に^^

---

# <span style="color: red">注意事項</span>
## AWS Casual だけど
## BigQuery のBK話をします

---

## 先にまとめ
### 前述の通り我々は BigQuery を選択しました
### スモールデータほど BigQuery は活用できます
### ほんとうに <span style="color: red">Big</span> になってきたら <span style="color: red">Redhift</span> か <span style="color: red">Hadoop おじさん</span>を採用しましょう

---

# 対象のサービス
## http://mtburn.jp/

___

## メディア企業の皆様
# どしどし<span style="color: red">お申込み</span>
# ください

___

# 再び
# 社会人としての
# 勤めを<span style="color: red">果たした</span>

---

## 課題
いままで Hadoop おじさんたちにお願いしていたログ集計を開発者側でコントロールしたい

でもログ置場や集計基盤の設計構築に悩みたくない<span style="color: red">わがままボディ</span>

---

## というのを解決したい

---

# ログ置場
## S3で十分

___

# ログ集計
## S3から読んで〜
## というのはツライ

___

# ログ集計
## 集計対象は増えるけど
## エンジニアはすぐに<span style="color: red">増えない</span>

___

# 候補に挙げたやつ
## ~~EMR~~
## Redshift
## BigQuery

---

# Redshift
## fast
## cost effective
## in less ops

___

# Redshift
- (ちゃんと設計すれば)fast
- (ちゃんと設計すれば)cost effective
- (ちゃんと設計すれば)in less ops

___

# Redshift
## SORTKEY -> 時間
## DISTKEY -> さてどうしよう

___

## fluent-plugin-redshift
### fluentd -> S3 -> Redshift

# <span style="color: red">冗長感</span>

---

# BigQuery
## modestly fast
## pay per use
## no ops

___

# BigQuery
- (何も考えずとも)modestly fast
- (何も考えずとも)pay per use
- (何も考えずとも)in less ops

___

## fluent-plugin-bigquery
### fluentd -> BigQuery

# <span style="color: red">シンプル</span>

___

## BigQuery
最近 timestamp 型をサポート

fluent-plugin-bigquery も 0.2.4 からサポート

![ts](images/schema.png)

---

# Compare
|  cmp   |  Redshift   |     BigQuery     |
|:------:|:-----------:|:----------------:|
| speed  |    fast     |     modestly     |
|  cost  | controlable |  uncontorolable  |
|  ops   |  requeired  |       free       |
| import | inefficient |    nefficient    |

---

# BigQuery :)
## そして
## 我々は BigQuery を選んだ
## これが<span style="color: red">辛い戦い</span>の
## 始まりだとも知らず

---

# BigQueryェ...

___

## "500 Backend Error"
出まくる

とにかく出まくる

___

## "500 Internal Error"
時々でる

思い出したように出る

___

## "500 Error"
謎のメッセージ

![err](images/bq-api-error.png)

___

## "500 Error"
API コールに HTML を返してくる

![500](images/google500.png)

___

# おちゃめ♡
___

# ここまでは
# <span style="color: red">リトライ</span>で
# 問題ない

___

# 413
# Request
# Too
# Large

___

## "413 Request Too Large"
![tl](images/toolarge.png)

___

# 失敗した
# 失敗した
# 失敗した

___

# 結果
![away](images/away.png)

___

# <span style="color: red">消</span>える＼(^o^)／

___

# 対策1
- 発生しづらいパラメタに調整
 - http://qiita.com/najeira/items/74799a67ac21c6b13415
 - こうしてから起こりにくくなりました

___

# 対策2
- 欠落したログを S3 から復帰させる
 - そういうスクリプトを書いておく

___

# 欠点
- ログの重複が許されない用途ではこの対策は不可能

---

# 結果
数々の<span style="color: red">アレ</span>な状況を乗り越えて

今は割と不自由なく<span style="color: green">お気楽ログ集計ライフ</span>を送っています

---

## まとめ
### 現在は BigQuery を選択して利用しています
### そんなに Big じゃない人たちには BigQuery 楽でいいかと思います
### ほんとうに <span style="color: red">Big</span> になって設計や細かい管理が必要になってきたら <span style="color: red">Redhift</span> か <span style="color: red">Hadoop おじさん</span>を採用しましょう

___

# おわり
