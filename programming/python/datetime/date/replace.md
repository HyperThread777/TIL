# replace
## 情報ソース
- https://docs.python.org/ja/3.13/library/datetime.html#datetime.date.replace

## 書式
- date.replace(year=self.year, month=self.month, day=self.day)

## 意味
指定されたパラメータを更新した、同じ値を持つ新しい日付オブジェクトを返す。  

## 実行例
### コード
```python
from datetime import datetime

now_replace = datetime.now().replace(hour=0, minute=0, second=0, microsecond=0)
print(f"現在時刻: {now_replace}")
```

### 出力内容
```log
現在時刻: 2025-09-13 00:00:00
```

### now_replaceの型
- datetime