# strptime

## 情報ソース
- https://docs.python.org/ja/3.5/library/datetime.html#datetime.datetime.strptime

## 書式
- classmethod datetime.strptime(date_string, format)

## 説明
`date_string` に対応した `datetime` を返します。  
`format` にしたがって構文解析されます。  

## 使い方
`Polars` では文字の列に対して使う  
```python
df = df.with_columns(
    pl.col("when")                              # ← 文字の列（"2025-09-01 08:00" など）
      .str.strptime(pl.Datetime, strict=False)  # ← 文字→日時 に変換
      .alias("when_dt")
)
```
- `str.strptime`: “string parse time” の略。**文字を日時に読み替える魔法**。
- `pl.Datetime`: 変換したい**型（日時）**を指定。
- `strict=False`: **やさしいモード**。変な文字はエラーにせず **Null（欠損）** にする。
    - `strict=True`（デフォルト）だと、読めない文字があると **エラー**になります。

# コツ（よく使うオプション）
- フォーマットが決まっているなら教えてあげると**確実**
```python
pl.col("when").str.strptime(pl.Datetime, fmt="%Y/%m/%d %H:%M", strict=False)
```
- 例：`"2025/09/01 08:00"` みたいな形を読む。

- **もし日付だけ**にしたいなら `pl.Date` を使う
```python
pl.col("day").str.strptime(pl.Date, fmt="%Y-%m-%d", strict=False)
```

## まとめ
- **何をする？**
    - 文字の日時っぽいもの → **ほんものの日時型**に変える
- **`strict=False` の意味？**
    - 変換できない文字は **Null** にして、処理を止めない
- **どこで使う？**
    - ふつうは `with_columns` の中で、`pl.col("列名").str.strptime(...)` の形で使う