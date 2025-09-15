# glob
## 情報ソース
- https://docs.python.org/ja/3.13/library/glob.html#module-glob

## 概要
- Unix シェルで使われているルールに従い指定されたパターンに一致するすべてのパス名を見つけ出す。
- `glob(...)` は 見つかったファイルの一覧（Pathのならび）をくれる“箱” 。
    - まとめて見たいときは `list(model_path.glob("lgb_*.txt"))` のようにリストに変える。

## 例文
### コード
```python
from pathlib import Path

model_path = Path("/workspaces/StockTrade/stock_price_forecasting/data/raw")

# ディレクトリ直下の .parquet を全部取得
for p in model_path.glob("*.parquet"):
    print(p)
```

### 出力
```log
/workspaces/StockTrade/stock_price_forecasting/data/raw/listed_info.parquet
/workspaces/StockTrade/stock_price_forecasting/data/raw/statement_range.parquet
/workspaces/StockTrade/stock_price_forecasting/data/raw/price_range.parquet
/workspaces/StockTrade/stock_price_forecasting/data/raw/weekly_margin_range.parquet
/workspaces/StockTrade/stock_price_forecasting/data/raw/short_selling_range.parquet
/workspaces/StockTrade/stock_price_forecasting/data/raw/trading_calendar.parquet
```

