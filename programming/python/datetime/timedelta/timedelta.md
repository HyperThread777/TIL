# timedelta
## 情報ソース
- https://docs.python.org/ja/3.13/library/datetime.html#timedelta-objects

## 書式
- class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)
- 全ての引数がオプションで、デフォルト値は0です。 引数は整数、浮動小数点数でもよく、正でも負でもかまいません。

## 意味
`timedelta` は、「どれだけ時間をずらすか」を表すための**時間のかたまり**です。  
「3日」「2時間30分」「1週間」みたいな“長さ”を作って、日付 (`datetime`) に足したり引いたりできます。  

## 実行例
### コード
```python
from datetime import datetime, timedelta

# 現在時刻
now = datetime.now()

# 3日前
three_days_ago = now - timedelta(days=3)

# 2時間後
in_two_hours = now + timedelta(hours=2)

# 1週間と2日後
future = now + timedelta(weeks=1, days=2)

print(f"現在時刻\t\t: {now}")
print(f"3日前\t\t\t: {three_days_ago}")
print(f"2時間後\t\t: {in_two_hours}")
print(f"1週間と2日後\t: {future}")
```

### 出力内容
```log
現在時刻		: 2025-09-13 15:28:49.020843
3日前			: 2025-09-10 15:28:49.020843
2時間後			: 2025-09-13 17:28:49.020843
1週間と2日後	: 2025-09-22 15:28:49.020843
```

### now_replaceの型
- datetime