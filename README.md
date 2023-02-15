# hotpepperAPI

PythonでAPIから取得したデータをCSVファイルに出力する方法
■使用モジュール
requests：APIにHTTPリクエストを送信
pandas：テーブル形式のデータを扱うためのデータ構造、Web APIからのデータ取得・CSV出力等
■データ処理の流れ・ポイント
API取得　⇒　json変換　⇒　※多次元配列のjsonをフラットな表形式に変換　⇒　CSV出力
※json_normalize関数でいいかんじに変換してくれる

■使用モジュール宣言
import requests
import pandas as pd

■データ処理の流れ（データ形式の変遷）
# APIからデータを取得（　response = requests.get(リクエストURL,検索クエリ)　）
#レスポンスをjson変換（　json_data = response.json()　）

{
'results': {
　　'api_version': '1.26',
　　'results_available': 22,
　　'results_returned': '22',
　　'results_start': 1,
　　｝,
　　'shop': [{----------------------------------------------ここからshop情報
　　　　'name': '来々軒',　　　　⇒一次元？配列
　　　　'lat': '35.2314646377',
　　　　'lng': '139.7033002565',
　　　　'genre': {　　　　　　　　⇒二次元配列
　　　　　　'code': 'G007 
　　　　　　'name': '中華'
　　　　},
　　　　'photo': {'　　　　　　　⇒三次元配列
　　　　　　'pc': {
　　　　　　　　'l': 'xx-l.jpg',
　　　　　　　　'm': 'xx-m.jpg'
　　　　　　　　's': 'xx-s.jpg'
　　　　　　},
　　　　},
....

# jsonからshopデータを取得（　shop= json_data['results']['shop']　）
# 多次元配列をpandasの表形式に変換（　df = pd.json_normalize(stores,sep='_')　）

(id)	name	lat	lng	genre_code	genre_name	photo_pc_l	photo_pc_m	photo_pc_s
0	来々軒	35.2314646377	139.7033002565	G007	中華	xx-l.jpg	xx-m.jpg	xx-s.jpg
# ＣＳＶ出力（　 df.to_csv('shop.csv',index=False)　）
name,	lat,	lng,	genre_code,	genre_name,	photo_pc_l,	photo_pc_m,	photo_pc_s
来々軒,	35.2314646377,	139.7033002565,	G007,	中華,	xx-l.jpg,	xx-m.jpg,	xx-s.jpg,
# これはコードとして書式設定されます
[ ]
編集するにはダブルクリックするか Enter キーを押してください

