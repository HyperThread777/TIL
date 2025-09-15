# sorted
## 情報ソース
- https://docs.python.org/ja/3.7/library/functions.html#sorted

## 書式
- sorted(iterable, *, key=None, reverse=False)

## 概要
- **「ならんでいるものを、きれいな順番にならべ直して“新しいリスト”をくれる」**

## 使用例
### コード
```python
nums = [3, 1, 2]
new_nums = sorted(nums)

print(new_nums)
print(nums)
```

### 出力
```log
[1, 2, 3]
[3, 1, 2]
```

## まとめ
- `sorted()` = ならべ替えロボ、新しいリストを返す
- `reverse=True` でさかさま
- `key=` で「どこを見て並べるか」を決められる（`len` が超よく使う合図）
