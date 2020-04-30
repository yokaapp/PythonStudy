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

上記の方法だと、条件が複数の場合に分かりにくいのと、書きづらい。

複数条件の場合は、df.query()を使うとよい。  
絞り込み条件を文字列で渡せる。
```
df.query('Height >= 165 & Weight >= 55')

# 条件に文字列を使うならダブルクオーテーションで囲う
df.query('Height >= 165 & Name == "佐藤"')

# 変数をquery内で使うなら@を付ける
target_name = '佐藤'
df.query('Height >= 165 & Name == @target_name')
```



## DataFrame

- DataFrame.loc[行ラベル]と行ラベルを指定すると、指定行のデータをシリーズとして取得できる。
- DataFrame.iloc[行番号] と行番号を指定すると指定行のデータをシリーズで取得できる。
- DataFrame.loc[行ラベル, 列ラベル] と指定すると、特定のデータを取得できる。
    - DataFrame.iloc[行番号, 列番号]でも絞り込める。

行ラベルの変更方法
- df.index = 新しい行ラベルのリスト：新しい行ラベルを設定する。
- df.reset_index(inplace=True)：行ラベルを通し番号に再設定する。元の行ラベルは、indexという名前の列で追加される。追加したくない場合は、drop=Trueをつける。
- df.set_index(インデックスにしたい列名, inplace=True)：指定した列を行ラベルに設定し、列から削除する。削除したくない場合は、drop=Falseをつける。

### DataFrame.itertuples()
DataFrame.itertuples()は、DataFrameの中の行を1行ずつ取得する。  
取得した行は、row.列名のようにして使える。  
データ数が多いとfor文の処理が遅くなる。



## Series

Series.strという形式でstrアクセサを利用できる。

```
# 文字列の長さ
df['CommentLen'] = df['Comment'].str.len()
```


### Series.apply()
引数として実装した関数を渡すと、Seriesの各データが実装した関数の引数として渡された結果をシリーズとして返す。  
一括で処理されるため、forで繰り返し処理するよりも実行速度があがる。
```
df['SlicedComment'] = df['Comment'].apply(get_sliced_str)
```

## DataFrameの結合
concatメソッドを使う。  
横に連結したい場合は、concatにaxis=1を指定する。  
DataFrame同士の連結はそれなりに実行速度がかかる。listなどで作成してからDataFrameに変換した方が高速になる。
```
# 2つの売上データを縦に結合
df3 = pd.concat([df1, df2])
df3 = pd.concat([df1, df2], ignore_index=True) # indexを連番にする

# 2つのデータを横に結合
df6 = pd.concat([df3, df5], axis=1)
```
