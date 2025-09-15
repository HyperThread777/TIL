# schema

## 情報ソース
- https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.schema.html

## 概要
- **表（DataFrame）に「"Date" 列って、どんな種類のデータ？」と聞いて、その答え（型）を `dtype` にメモする**

## 何が入るのか
Polars では列の“種類（型）”をこんな感じで表す
- `pl.Date`: 日付だけ（カレンダーの⽇）
- `pl.Datetime`: 日付＋時刻（時計つき）
- `pl.Utf8`: 文字（テキスト）
- `pl.Int64`: 整数
- `pl.Float64`: 小数など

## 使い方
### コード
```python
import polars as pl
from datetime import date

df = pl.DataFrame({
    "Date": [date(2025, 9, 1), date(2025, 9, 2)],
    "Name": ["Taro", "Hanako"]
})

dtype = df.schema["Date"]
if dtype == pl.Date:
    print("Date列は日付")
elif dtype == pl.Datetime:
    print("Date列は日付 + 時間")
```

### 出力
```log
Date列は日付
```

## 補助ワザ
- **列がないとエラー**になるので安全に取りたいとき
```python
dtype = df.schema.get("Date")  # 見つからなければ None
if dtype is None:
    print("Date 列がない")
```

- **型を直したい**（たとえば文字→日付）
```python
df = df.with_columns(
    pl.col("Date").str.strptime(pl.Date, fmt="%Y-%m-%d")
)
```
覚え方：`df.schema` は「列名 → 型」の辞書
`["Date"]` で「Date列の型ちょうだい！」と取り出している、というわけです。
