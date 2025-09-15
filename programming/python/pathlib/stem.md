# stem

## 概要
- パス要素の末尾から拡張子を除いたもの

## 使い方
### コード
```python
from pathlib import Path

file_path = Path("/models/lgb_001.txt")

print(f"name:\t{file_path.name}")
print(f"stem:\t{file_path.stem}")
print(f"suffix:\t{file_path.suffix}")
```

### 出力
```log
name:	lgb_001.txt
stem:	lgb_001
suffix:	.txt
```

## 何に使うのか
“土台の名前”があると、関連ファイルを作りやすい。  

### コード
```python
from pathlib import Path

file_path = Path("/models/lgb_001.txt")

print(f"log file:\t{file_path.stem}.log")
print(f"bak file:\t{file_path.stem}.bak")
```

### 出力
```log
log file:	lgb_001.log
bak file:	lgb_001.bak
```

## 注意点
- 拡張子が2つある名前（例: `model.tar.gz`）だと `Path("model.tar.gz").stem` は **'model.tar'**
    - いちばん最後の `.gz` だけが取れる
- ふつうに使うぶんには「**拡張子を1個はずす**」と思っておけばOK！