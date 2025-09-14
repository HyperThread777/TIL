# perf_counter
## 情報ソース
- https://docs.python.org/ja/3.7/library/time.html?highlight=time%20perf_counter#time.perf_counter

## 概要
`time.perf_counter()` は「**パソコンの中の超正確なストップウォッチ**」。  
そのストップウォッチの**今の値を“スタート時刻”としてメモ**してる。  

## 使い方
「この作業、どのくらい時間がかかった？」を知りたいときに使う。
あとでもう一回 `time.perf_counter()` を呼んで、引き算すると**かかった秒数**がわかる。

## 例
### コード
```python
import time

t0 = time.perf_counter()        # スタート
time.sleep(1.2)                 # 1.2秒待つ
dt = time.perf_counter() - t0   # ゴール

print(f"かかった時間: {dt:.3f}秒")
```

### 出力
```log
かかった時間: 1.200秒
```

## ポイント
- 返ってくる数は秒（小数点あり）
- とても精密で、時間が戻らない（時計の合わせ直しの影響を受けない）から、測定にピッタリ
- 現在時刻（何年何月何日）を知るものじゃない
- 経過時間を測るための道具