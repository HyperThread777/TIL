---
title: "Optinalについて"
date_created: 2025-08-14
tags:
  - python
  - optinal
---

# 概要

Pythonのキーワードである`Optional`についてまとめ。  

# 詳細
Python の `Optional` は**その値が `None` かもしれない**ことを型注釈で表すための記法。  
その中身はただの **Union[T, None]**（T または None）。  

## 1. 基本
- `Optional[T]` は **T または None** の意味。
- Python 3.10+ では、より読みやすい `T | None` が推奨。

```python
from typing import Optional

name: Optional[str] = None
age: int | None = 20        # 3.10+で推奨
```

## 2. いつ使うのか？
- 引数が**省略可能**で、デフォルトを `None` にしたいとき
- 関数の**戻り値が見つからない場合に None** を返しうるとき
- 外部ライブラリ等で **None** が入りうるデータを扱うとき

```python
def find_user_email(user_id: int) -> str | None:
    # 見つからなければ None を返す
    ...

def greet(name: str | None = None) -> None:
    if name is None:
        print("Hello!")
    else:
        print(f"Hello, {name}!")
```

## 3. Noneの扱い（ガード必須）
`Optional` を受け取ったら、**使う前に None チェック**が必要。 

```python
def upper_or_default(s: str | None, default: str = "N/A") -> str:
    if s is None:
        return default
    # ここから s は str とみなせる
    return s.upper()
```
補足テク：
- 早期 return で分岐を**できるだけ手前で確定**させると読みやすくなる。
- どうしても後続で非 None を主張したいときは `assert s is not None` や `typing.cast` も使える（乱用は非推奨）。

## 4. よくある勘違い
- `Optional[str]` は **空文字 `""` を許可する**という意味ではない。**`None` を許す**だけ。  
  空文字や `0` は **“偽”** だが **`None` とは別**。

- `x or default` での置き換えは、`0` や `""` まで既定値に化ける。  
  `default if x is None else x` を使うと安全。

```python
def safe_default(x: int | None, default: int = 10) -> int:
    # NG: x が 0 でも default になる
    # return x or default

    # OK: None のときだけ default
    return default if x is None else x
```

## 5. コレクションでの違い（重要）
- `list[int | None]` … **要素**が None を含むリスト（例: `[1, None, 3]`）
- `list[int] | None` … **リスト全体**が存在しないかもしれない（例: `None` または `[1, 2]`）

```python
def f(a: list[int | None]) -> None:
    # a は常にリスト。中に None が混ざる可能性あり
    ...

def g(a: list[int] | None) -> None:
    # a 自体が None かもしれない
    if a is None:
        return
    ...
```

## 6. デフォルト引数との組み合わせ（実用パターン）
**ミュータブル（list, dict など）をデフォルト引数に直接置かない**ために、`None` を使うのが定番。

```python
from dataclasses import dataclass, field
from typing import Any

def add_item(item: Any, bucket: list[Any] | None = None) -> list[Any]:
    if bucket is None:          # ここで新しいリストを作る
        bucket = []
    bucket.append(item)
    return bucket

@dataclass
class User:
    nickname: str | None = None
    tags: list[str] | None = None

    def add_tag(self, tag: str) -> None:
        if self.tags is None:
            self.tags = []
        self.tags.append(tag)
```

## 7. 型チェックのイメージ
- `mypy`, `pyright` などの型チェッカーは、`Optional` を見ると**None のまま使ってない？**を警告します。
- 実行時に型制約はされません（**型注釈はあくまで静的情報**）。

## 8. まとめ（チートシート）
- `Optional[T]` ≒ `T | None`（3.10+ は後者推奨）
- 使う前に `if x is None:` で**必ずガード**
- `or` で既定値を入れるのは **None 以外も潰す**ので注意
- コレクションは **要素が Optional** と **コレクション自体が Optional** を区別
- ミュータブル既定値は `None` を使って**初回に生成**
