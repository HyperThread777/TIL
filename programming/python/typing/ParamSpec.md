# ParamSpec
## 情報ソース
- https://docs.python.org/ja/3/library/typing.html#typing.ParamSpec

## 書式
- class typing.ParamSpec(name, *, bound=None, covariant=False, contravariant=False, default=typing.NoDefault)

## 意味
**「関数がどんな引数をもらうか」という“レシピ”を丸ごと箱に入れて覚えておく道具**。  
`P = ParamSpec("P")` は、その箱に P という名前を付けているだけ。

## 必要性
**別の関数の中で"だれかの関数"を呼ぶときに元の関数の引数の形（何個、どんな型）をそのまま受け渡したいことがある**。  
**`ParamSpec`を使うと、その引数の並び**をちゃんと覚えて、`*args`や`**kwargs`に綺麗に引き継げる。  

### 例
- **TypeVar** = できあがる"ケーキの種類"（戻り値の型など）
- **ParamSpec** = ケーキを作る"材料のレシピ"（引数の並び）

## 実装例
```python
from typing import Callable, ParamSpec, TypeVar

P = ParamSpec("P")  # 引数レシピの箱に名前をつけた
R = TypeVar("R")    # 戻り値の型

def call_twice(
    fn: Callablep[P, R],
    *args: P.args,
    **kwargs: P.kwargs
) -> R:
    # fn がどんな引数を取るか（P）をそのまま受け取って2回コール
    fn(*args, **kwargs)
    return fn(*args, **kwargs)
```
- `Callable[P, R]`: 引数はP（レシピ通り）、戻り値はR
- `*args: P.args, **kwargs: P.kwargs`: Pで覚えた引数たちをそのまま受け取る
IDEでも型チェッカーでも、**fnの引数の型がちゃんと伝わる**のがメリット

## まとめ
- `P = ParamSpec("P")` は「**関数の引数レシピ**に P という名前をつける」こと
- `TypeVar` が“出来上がりの型”なら、`ParamSpec` は“材料リスト”
- デコレータやユーティリティ関数で、**元の関数の引数情報を失わずに引き回せる**のが嬉しいポイント

---

# `args`, `kwargs`について
## 概要
- `*args`は「**順番だけ渡す荷物のならび**」 = タプル（例: `(10, 20)`）
- `**kwargs`は「**名前付きの荷物**」 = 辞書（例: `{"x": 10, "y": 20}`）
関数の中では、`df = fn(*args, **kwargs)`で「順番の荷物」と「名前付きの荷物」をそのまま `fn` に渡して呼び出す。  

## 具体例
- 例1: 引数なし
```python
fetch(cli, "何かを取得", fn)    # fn()
```
- `args`は`()`（空っぽ）
- `kwargs`は`{}`（空っぽ）

- 例2: 位置引数（順番で渡す）
```python
fetch(cli, "計算", fn, 10, 20)  # fn(10, 20)
```
- `args == (10, 20)`
- `kwargs == {}`
- 「10が1番目、20が2番目」という**順番だけ伝わる**

- 例3: キーワード引数（名前で渡す）
```python
fetch(cli, "計算", fn, x=10, y=20)  # fn(x=10, y=20)
```
- `args == ()`
- `kwargs == {"x": 10, "y": 20}`
- **名前つき**だから順番は関係ない

- 例4：まぜこぜ（位置 + キーワード）
```python
fetch(cli, "計算", fn, 10, y=20)      # ← fn(10, y=20)
```
- `args == (10,)`
- `kwargs == {"y": 20}`
