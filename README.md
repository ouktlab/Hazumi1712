# Hazumi1712
Osaka University Multimodal Dialogue Corpus (Hazumi1712)

Hazumi1712では以下のファイル群を公開している．
```
1. ビデオデータ
2. MS Kinectデータ
3. 閲覧用ELANファイル
4. 実験用ダンプファイル
```
このうち，ここでは3.と4.を公開している．

1.と2.は，データ利用に関する契約を交わした利用者のみに対して，NII IDR
（国立情報学研究所 情報学研究データリポジトリ）から配布される．
データの入手方法や概要説明ドキュメントは，NII IDRサイトを参照のこと．  
https://www.nii.ac.jp/dsc/idr/rdata/Hazumi/  
https://www.nii.ac.jp/dsc/idr/rdata/Hazumi/documents/HazumiOverviewInPerson.pdf

## 閲覧用ELANファイル
アノテーションや書き起こしを全て含んだeaf（ELAN annotation fomat）ファイ
ルである．
アノテーションツールELANで，実験参加者ビデオを読み込んで使用する．

elan/ 以下に，(実験参加者ID).eaf という名前で置かれている．実験参
加者IDは以下に示すYYMMGAANNの9文字である．  
　YYMM	コーパスのバージョン．1712もしくは1902．  
　G	実験参加者の性別．M（男性）もしくはF（女性）．  
　AA	実験参加者の年代．20から70まで．  
　NN	上記7文字と合わせてIDとなるように付番．  
例えば1712F2006は「Hazumi1712の，20歳代のある女性のデータ」を意味する．

交換と呼ばれる単位ごとに，以下で示されるデータが付与されている．
詳しくは概要説明書を参照のこと．

#### 実験参加者の発話の書き起こし
#### システム発話とその対話行為
#### 心象アノテーション
#### 話題継続アノテーション
#### 興味度アノテーション

## 実験用ダンプファイルの概要:
対話中のユーザ（実験参加者）の心象を含むラベルデータを予測するためのマルチモーダル機械学習実験を行うための利用を想定している．
このファイルは， 機械学習の入力として，ユーザの音声，映像，音声認識の結果から得られる発話内容（言語）から抽出したマルチモーダル特徴量（実数値），機械学習の出力として興味度・心象・話題継続アノテーションの値（連続値とカテゴリ値）を格納している．

実験用ダンプファイルは以下の書誌に掲載された研究で使用されたものです．  
```
Yuki Hirano, Shogo Okada, Haruto Nishimoto, and Kazunori Komatani. 2019. Multitask Prediction of Exchange-level Annotations for Multimodal Dialogue Systems. In 2019 International Conference on Multimodal Interaction (ICMI ’19). Association for Computing Machinery, New York, NY, USA, 85–94. DOI:https://doi.org/10.1145/3340555.3353730
```

### フォルダの構造とファイル：
dumpfiles    
|---------	1712F2006.csv  
|---------	1712F2010.csv  
:  
:  

各ファイル（1712F2006.csv）は各セッションにおける参加者の発話対ごとのマルチモーダル特徴量とアノテーションの値を含む．
データ欠損のある1712F4002, 1712M5003に関して，
ダンプファイルは存在しない．
計27人分のダンプファイルを格納している．

### 実験用ダンプファイル中のデータ説明：
Hazumi1712のデータセット：
各列はデータの属性，各行は1発話対で抽出された各特徴量の数値を格納している．
1行目はデータの属性名を示す．


#### start(exchange), end(system), end(exchange):
ユーザの上半身動画（mp4）におけるシステム, ユーザの発話開始, 終了時間 

#### kinectstart(exchange), kinectend(system), kinectend(exchange): 
kinectデータにおけるシステム, ユーザの発話開始, 終了時間 (kinect)

#### IN_ternary, TC_ternary, TS_ternary : 
interest（興味度ラベル）, topic continuance（話題継続ラベル）, third sentiment（第3者により付与されたユーザ心象ラベル）のアノテーションの値 (IN,TC,TS)を，それぞれ三段階でラベル化したもの. 2が高群, 1がニュートラル, 0が低群. 

##### 3段階のラベル化方法：
Interestの場合: (o)を1, (t)を0, (x)を-1としてアノテーションの合計値を計算して1より高ければ高群, -1より低ければ低群, それ以外は中間群と決定.  
Topic continuanceとThird sentimentの場合アノテータの平均値が4.5より高ければ高群, 3.5より低ければ低群, それ以外は中間群と決定. 

#### IN1~IN6: 
6名により付与されたinterestのアノテーション値　（アノテータが3人の場合もある．）

#### TC1~TC5:
5名により付与されたtopic continuanceのアノテーション値.

#### TS1~TS5:
5名により付与されたthird sentimentのアノテーション値．

#### pcm_RMSenergy_sma_max ~ F0_sma_de_kurtosis: 
OpenSmileによって抽出された韻律特徴量. Config fileはIS09_emotion.conf.

#### word#001~su： 
ユーザとシステムの発話から抽出された言語特徴量.
Google Speech APIを用いて得られたユーザ発話の音声認識結果から特徴量を抽出した．日本語形態素解析器Mecab\cite{mecab}を使用して形態素解析を行い，1発話内に含まれる品詞ごとの単語数および各単語の出現回数を表すBag-of-Words（BoW）を言語特徴量として使用した．
またユーザの応答時間，システム発話のタイプといった特徴量も含まれる．

#### 17_acceleration_max~AU45_c_mean: 
顔表情特徴量. 「17」のような数字はopenfaceのlandmarkを示す. AUはaction unit.
landmark特徴量はOpenfaceから得られる2次元座標データ(30fps)をもとに目の周り, 口の周りなどの12点のフレーム間速度, 加速度をexchange区間ごとに求め最大値, 平均値, 標準偏差を特徴量とする. またexchange区間のAUの有無の平均を特徴量とする. 

#### HandLeft_acceleration_max~duration: 
動作特徴量. Durationは発話時間
Kinectの3次元間座標データ(30fps)をもとに頭部, 肩などの部位のフレーム間速度, 加速度を求め, exchange区間ごとに最大値, 最小値, 平均値を特徴量とする. 


### マルチモーダルデータの同期方法：
ユーザの音声データ・姿勢データはKinect V2センサ (Kinect)より，RGB画像データはビデオカメラより取得した．
Kinectのデータ記録時間とビデオカメラのデータ記録時間において，各セッションのエージェント発話開始点を特定して遅延を計算し，両デバイスから得られたデータの時刻同期を行った．またMMDエージェントの発話ログファイルを用いて，
ユーザの発話区間・エージェントの発話区間の切り分けを行った．
言語特徴量（素性）は発話区間に対応する音声データに対して音声認識を行うことでテキスト変換した後に抽出した．上記の手順で，音声・言語・画像の情報の時間同期を行った．
 詳しくは概要説明書を参照のこと．


# Authors
* 駒谷 和範（大阪大学 産業科学研究所） komatani@sanken.osaka-u.ac.jp
* 岡田 将吾（北陸先端科学技術大学院大学） okada-s@jaist.ac.jp
