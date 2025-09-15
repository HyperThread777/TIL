# sort

## 情報ソース
- https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.sort.html

## 実装例
### コード
```python
import polars as pl

# 日付がバラバラに並んでいる状態
df = pl.DataFrame({
    "Date": ["2025-09-01",
             "2025-09-03",
             "2025-09-02"],
    "x": [1, 2, 3]
})

# 文字から日付にしてから並べ替える
df = df.with_columns(pl.col("Date").str.strptime(pl.Date, "%Y-%m-%d"))
df = df.sort("Date")

print(f"{df}")
```

### 出力
```log
shape: (3, 2)
┌────────────┬─────┐
│ Date       ┆ x   │
│ ---        ┆ --- │
│ date       ┆ i64 │
╞════════════╪═════╡
│ 2025-09-01 ┆ 1   │
│ 2025-09-02 ┆ 3   │
│ 2025-09-03 ┆ 2   │
└────────────┴─────┘
```

## よくあるアレンジ
- **逆順（新しい→古い）**にする
```python
df = df.sort("Date", descending=True)
```

- **同点のならび替え鍵を足す**（日付が同じときは Codeで）
```python
df = df.sort(["Date", "Code"])
```

- **欠損（null）をいちばん後ろに**
```python
df = df.sort("Date", nulls_last=True)
```

## 覚え方
- `sort` = **並べ替えロボ**
- 「どの列で並べる？」→ `"Date"`
- 「どっち向き？」→ `descending=`
- 「穴（null）は？」→ `nulls_last=` を決めるだけ！