# 役に立つ繰り返し構文

## イテラブルとイテレーター

イテラブルとは、繰り返しに使えるオブジェクト。
list, dict, set, strクラスのオブジェクトはイテラブル。

変数itrがイテラブルのとき
- iter(itr)がイテレーターを返す。
    - iter(itr)は、iter.__iter__()を呼ぶので、__iter__メソッドを持っていなければならない。
- isinstance(itr, typing.Iterable)がTrueになる。

変数itがイテレーターのとき
- イテレーターはイテラブルの一種。
    - __iter__メソッドを持っておく必要がある。__iter__メソッドで自分自身を返す。
- オブジェクト作成時に、内部的に最初の要素を指すようにする。
- next(it)で現在の要素を返し、次の要素に進める。
    - next(it)はit.__next__()を呼ぶので、__next__メソッドを持っていなければならない。
    - もし現在の要素がなければ、StopIteration例外を発生させる。
- isinstance(it, typing.Iterator)がTrueになる。
    - isinstance(it, typing.Iterable)もTrueになる。

## ジェネレーターとは

イテレーターの一種で、より簡単に繰り返しができるオブジェクト。

- yield 返り値　を含む関数を定義する
    - その関数を実行すると、ジェネレーターになる
- ジェネレーターはイテレーターの一種だが、イテラブルとして使うことが多い
- isinstance(ジェネレーター, typing.Generator)がTrueになる。
    - isinstance(it, typing.Iterator)もTrueになる。
    - isinstance(it, typing.Iterable)もTrueになる。

**遅延評価**
- ジェネレーターオブジェクトを受け取っても、評価はされない
- ジェネレーターに対し、nextを呼ぶごとにyieldまで実行する
- 一連のジェネレーターが途中で終了すると、全ジェネレーターの処理も止まる。

遅延評価のメリット
- 初期化に時間がかからない
- 無駄な処理をしない

yield fromについて

- yield from イテラブルという形で使う。下記と同じ処理になる。
~~~
for i in イテラブル:
    yield i
~~~
- yield from を使うと、繰り返し処理をfor文を書かずにできる。再帰処理や別の関数に移譲したいときに便利。
