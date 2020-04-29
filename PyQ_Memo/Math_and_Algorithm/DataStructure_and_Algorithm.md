# 数学とアルゴリズム

## データ構造（ヒープ）

ヒープ（heap）は、二分木といわれる構造で、以下の特徴があります。
- 親（矢印の先）は、子供（矢印の元）より小さい。
- 子供は、高いところから順番に埋まる。高さが同じなら左から埋まる。

下記などのアルゴリズムで、利用されます。
- ヒープソート（ヒープを使ったソート）
- ダイクストラ法（最短路を求めるアルゴリズム）
- クラスカル法（最小全域木を求めるアルゴリズム）

ヒープはソートで使われることが多いです。しかし、ソートしたいだけであればsortedなどでソートする方が効率的です。
ただし、小さい方から数個だけ欲しいときは、ソートせずにヒープを使った方が効率的になります。

目的別の効率的な方法を以下にまとめます。括弧内は利用できる関数やモジュールです。

- 一番小さいものだけ欲しいとき：全て調べます（min関数）
- 小さい方から、数個欲しいとき：ヒープ構造にして、欲しい分だけ取り出します（heapqモジュール）
- 小さい方から、たくさん欲しいとき：ソートします（sorted関数など）


## データ構造(素集合:UnionFind)

データの集合を素集合に分けて管理する。
UnionFindの機能
- Union: 2つの要素を同じ集合にする（親を同一にする）
- Find: ある要素が所属する集合の代表値を求める。この値を2つの要素で比較すれば、同じ集合に所属しているかどうかがわかる。

UnionFindを使うことで、グラフが連結かどうかを確認することなどができる。

演習５問目
```
from collections import defaultdict

class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))

    def find(self, i):
        while self.parent[i] != i:
            i = self.parent[i]
        return i

    def union(self, i, j):
        i = self.find(i)
        j = self.find(j)
        self.parent[j] = i


data = [(0, 2), (4, 1), (3, 2)]  # 辺のリスト
d = defaultdict(list)  # グルーピングを入れる辞書
```

```
u = UnionFind(5)
for i, j in data:
    u.union(i, j)

for i in range(5):
    d[u.find(i)].append(i)

d.values()
```

