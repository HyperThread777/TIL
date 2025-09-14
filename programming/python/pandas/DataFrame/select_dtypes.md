# select_dtypes

## 情報ソース
- https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.select_dtypes.html

## 記法
- DataFrame.select_dtypes(include=None, exclude=None)
- `include`: ほしい型
- `exclude`: いらない型
- どちらか一方だけでもOK。両方同時もOK。

## 概要
**表（DataFrame）の列を“データの種類（型）でふるい分けする道具”**。  
「数字だけ」「文字だけ」「日付だけ」みたいに、ほしい列だけスッと取り出せる。  

## 実装例
### コード
```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "age": [10, 12],                                        # 数字
    "name": ["Taro", "Hanako"],                             # 文字（object）
    "score": [95.0, 88.0],                                  # 小数
    "is_pass": [True, False],                               # True/False
    "when": pd.to_datetime(["2025-09-01", "2025-09-02"])    # 日付
})

df_text = df.select_dtypes(include=["object", "string"])
print(f"文字列: \n{df_text}")

df_non_num = df.select_dtypes(exclude="number")
print(f"数字列以外: \n{df_non_num}")
```

### 出力
```log
文字列: 
     name
0    Taro
1  Hanako
数字列以外: 
     name  is_pass       when
0    Taro     True 2025-09-01
1  Hanako    False 2025-09-02
```

## 指定出来るパラメータ
- 便利なキーワード："number", "bool", "datetime", "datetimetz", "timedelta", "category", "object", "string"
- くわしい型名："int64", "float64", など
- NumPy の型：np.number, np.integer, np.floating などもOK

## 注意
- `object` と `string` は別物：
文字列を“ちゃんと文字”として扱いたいなら `string` dtype を使う列は `include=["object","string"]` のように両方指定。
- **欠損ありの整数**は `Int64`（大文字のI）など“nullable整数”になることがある。厳密に整数だけ欲しいときは注意。
- *戻り値は列を間引いた新しいDataFrame*。  
- 列名だけ欲しいなら `.columns` を使う：
```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "age": [10, 12],                                        # 数字
    "name": ["Taro", "Hanako"],                             # 文字（object）
    "score": [95.0, 88.0],                                  # 小数
    "is_pass": [True, False],                               # True/False
    "when": pd.to_datetime(["2025-09-01", "2025-09-02"])    # 日付
})

df_text = df.select_dtypes(include=["object", "string"]).columns
print(f"行: \n{df_text}")
```

```log
行: 
Index(['name'], dtype='object')
```