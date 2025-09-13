# now
## 情報ソース
- https://docs.python.org/ja/3.13/library/datetime.html#datetime.datetime.now

## 書式
- classmethod datetime.now(tz=None)

## 意味
現在のローカルな日時を返します。  
現在、何月何日、何時何分何秒かを取得する。  

## 実行例
### コード
```python
from datetime import datetime

now = datetime.now()
print(f"現在時刻: {now}")
```

### 出力内容
```log
現在時刻: 2025-09-13 14:11:22.769791
```

### nowの型
- datetime