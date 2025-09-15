# select

## 情報ソース
- https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.select.html

## 書式
```python
DataFrame.select(
    *exprs: IntoExpr | Iterable[IntoExpr],
    **named_exprs: IntoExpr,
) → DataFrame[source]
```

## 概要
**「この表から、ほしい列だけを取り出したり、計算して新しい“ミニ表”を作るボタン」**
※ ボタンなので、かっこを付けて押さないと動かない
- `df_new.select(...)`

## 基本の使い方
### コード
```python
import polars as pl

df = pl.DataFrame({
    "name": ["A", "B"],
    "price": [100, 200],
    "qty": [3, 5]
})

mini = df.select(["name", "price"]) # name と price だけのミニ表を作成
print(f"{mini}")
```

### 出力
```log
shape: (2, 2)
┌──────┬───────┐
│ name ┆ price │
│ ---  ┆ ---   │
│ str  ┆ i64   │
╞══════╪═══════╡
│ A    ┆ 100   │
│ B    ┆ 200   │
└──────┴───────┘
```

## よく使用するワザ
1. 全部の列から一部を除く
```python
import polars as pl

df = pl.DataFrame({
    "name": ["A", "B"],
    "price": [100, 200],
    "qty": [3, 5]
})

mini = df.select(pl.all().exclude("name"))   # name を除いた全部
print(f"{mini}")
```
```log
shape: (2, 2)
┌───────┬─────┐
│ price ┆ qty │
│ ---   ┆ --- │
│ i64   ┆ i64 │
╞═══════╪═════╡
│ 100   ┆ 3   │
│ 200   ┆ 5   │
└───────┴─────┘
```

2. 文字列だけ
```python
import polars as pl

df = pl.DataFrame({
    "name": ["A", "B"],
    "price": [100, 200],
    "qty": [3, 5]
})

mini = df.select(pl.col(pl.Utf8))   # name を除いた全部
print(f"{mini}")
```
```log
shape: (2, 1)
┌──────┐
│ name │
│ ---  │
│ str  │
╞══════╡
│ A    │
│ B    │
└──────┘
```

## `with_columns`とのちがい（大切）
- `select(...)`：**取り出したり計算した列“だけ”のミニ表を返す**
- `with_columns(...)`：元の表に**列を足す／作り直す**（他の列も残る）

### 例
```python
import polars as pl

df = pl.DataFrame({
    "name": ["A", "B"],
    "price": [100, 200],
    "qty": [3, 5]
})

df2 = df.__copy__()

# amount だけの表
df_select = df.select(
    (pl.col("price") * pl.col("qty")).alias("amount")
)
print(f"{df_select}")

# 元の列 + amount を持つ表
df_with_col = df2.with_columns(
    (pl.col("price") * pl.col("qty")).alias("amount")
)
print(f"{df_with_col}")
```

```log
shape: (2, 1)
┌────────┐
│ amount │
│ ---    │
│ i64    │
╞════════╡
│ 300    │
│ 1000   │
└────────┘
shape: (2, 4)
┌──────┬───────┬─────┬────────┐
│ name ┆ price ┆ qty ┆ amount │
│ ---  ┆ ---   ┆ --- ┆ ---    │
│ str  ┆ i64   ┆ i64 ┆ i64    │
╞══════╪═══════╪═════╪════════╡
│ A    ┆ 100   ┆ 3   ┆ 300    │
│ B    ┆ 200   ┆ 5   ┆ 1000   │
└──────┴───────┴─────┴────────┘
```

## よくある勘違い
- `df_new.select`（カッコなし）: **まだ押してない**から何もしない！
    - `df_new.select(...) `のように **( )** を付けて中身を書く
- `select` は**新しい表を返す**だけ。元の `df` は変わらない
    - 必要なら `mini = df.select(...)` と**代入**

## まとめ
`select` = **欲しい列だけの“ミニ表工場”**。  
列名で選ぶ・型で選ぶ・計算して作る…ぜんぶ OK。  
元の表はそのまま、新しい表が返ってくる。  