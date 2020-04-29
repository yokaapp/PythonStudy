# Pandasとは

Python製のデータ分析ツール  
データの読み込み、集計、加工、絞り込みなどの機能を利用したり、データ分析のための前処理で役立つ。

以下のデータ構造を利用する。
- Series  
1次元で、リストのような形式。表の1列分のデータを抜き出したもの。  
NumPyの多次元配列の一種である1次元配列を使っている。

- DataFrame  
２次元で表のような形式
列と行それぞれにラベルを持っている。
行ラベルは、df.index, 列ラベルは、df.columnsでアクセスできる。

以下のようにimportして使う。
```
import pandas as pd

df = pd.DataFrame([['佐藤', 28], ['田中', 28]], columuns=['Name', 'Age])

# 特定列の取り出し
ages = df['Age']

# 抽出した列の最大値等の取り出し。
max_age = ages.max() # 最大値
min_age = ages.min() # 最小値
mean_age = ages.mean() # 平均
sum_age = ages.sum() # 合計

```
pd.DataFrameでデータフレームを作成している。  
columnsで列名を指定している。その列名を使って、アクセスできる。  


# pandasでのデータ集計

## pandasでのデータ読み込み

pandas.read_ファイル形式()のように、read_のあとにファイル形式にしたがった名前を指定してファイルを読み込み、DataFarameに変換できる。

```
# csvファイルを読み込んでデータフレームに展開
df = pd.read_csv('dataset/stationery.csv', encoding='utf-8')

# 先頭の5行
df.head()
df[:5]

# 末尾5行
df.tail()
df[-5:]
```

## pandasでのデータ集計

特定列の集計  
df['列名'].value_counts()を使う
```
# 文房具の名前でcount
stationery_counts = df['Name'].value_counts()
```

## pandasでのグループ集計

DataFrame.groupby() を利用すると、グループごとの合計値、平均などを集計できる。

```
# 文房具の名前でグループ化した合計金額を表示
grouped_df = df.groupby(by='Name').sum()
total_price = grouped_df['Price']
```

## データの絞り込み

論理演算はandやor、notではなく、&と|と~を使う。

```
df_filtering = df[(df['BMI'] >= 25) | (df['BMI'] < 18)]
```


