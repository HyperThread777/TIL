# with_columns

## 情報ソース
- https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.with_columns.html

## 概要
**「表に新しい列を“足したり・作り直したり”する魔法ボタン」**。  
**押すと新しい表を返す**から、元の `df` はそのまま。  
使うときは**代入して受け取る**。

## 基本
### コード
```python
import polars as pl

df = pl.DataFrame({
    "price": [100, 200],
    "qty": [3, 5]
})

df2 = df.with_columns([
    (pl.col("price") * pl.col("qty")).alias("amount"),  # 新しい列 amount
    pl.lit("JPY").alias("currency"),                    # いつも同じ値の列を追加
])

print(df2)
```

### 出力
```log
shape: (2, 4)
┌───────┬─────┬────────┬──────────┐
│ price ┆ qty ┆ amount ┆ currency │
│ ---   ┆ --- ┆ ---    ┆ ---      │
│ i64   ┆ i64 ┆ i64    ┆ str      │
╞═══════╪═════╪════════╪══════════╡
│ 100   ┆ 3   ┆ 300    ┆ JPY      │
│ 200   ┆ 5   ┆ 1000   ┆ JPY      │
└───────┴─────┴────────┴──────────┘
```

## 良く使うワザ
1. 既存の列を作り直す（型をそろえる等）
```python
df2 = df.with_columns(
    pl.col("price").cast(pl.Float64).alias("price")   # price を小数型に
)
```

2. 文字 → 日付に変える
```python
df = pl.DataFrame({"Date": ["2025-09-01", "2025-09-02"]})
df2 = df.with_columns(
    pl.col("Date").str.strptime(pl.Date, fmt="%Y-%m-%d")
)
```

3. 条件でラベルをつける
```python
df = pl.DataFrame({"score": [95, 72, 40]})
df2 = df.with_columns(
    pl.when(pl.col("score") >= 80).then("good")
      .when(pl.col("score") >= 50).then("so-so")
      .otherwise("try!").alias("label")
)
```

## 大事なポイント
- **元の `df` は変わらない**
    - `df = df.with_columns(...)` のように**受け取り直す**
- **同じ中で作った新列をすぐまた使えないことがある**
    - そういう時は以下のように2回に分ける
    ```python
    df = df.with_columns( ... "tmp" ... )
    df = df.with_columns( pl.col("tmp") * 2 )   # 2回に分ける
    ```
- 追加も上書きも、**ぜんぶ“式（expression）”で書くのが Polars のコツ**

## 一言まとめ
`with_columns` = **新しい列を作る工房**
足す・直す・変換する → ぜんぶ式で書いて、**新しい表として受け取る**