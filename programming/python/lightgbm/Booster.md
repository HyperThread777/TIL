# Booster

## 情報ソース
- https://lightgbm.readthedocs.io/en/latest/pythonapi/lightgbm.Booster.html

## 概要
- **LightGBMの「学習済みモデル本体」**
- たくさんの決定木（木の質問マシン）がチームになって、みんなで力を合わせて予測（**“勉強して強くなる”= Boosting**）

## 使い方
```python
import lightgbm as lgb

model = lgb.Booster(model_file="lgb_001.txt")                   # 読み込み
y_pred = model.predict(X, num_iteration=model.best_iteration)   # 予測
```
- `X`：学習時と**同じ順番・同じ特徴量名**で用意（大事）
- `best_iteration`：一番よかった回数までの木を使って予測（過学習を防ぐ）

## Boosterって何者？
- **中身**：たくさんの決定木（“弱い選手”）を、順番に強化しながら**足し合わせた**“強いチーム”
- **できること**：`predict`、`save_model`（保存）、`feature_name`の確認 など
- **兄弟**：`LGBMClassifier/LGBMRegressor` は **scikit-learn 風の便利ラッパー**
  その中にも最終的には **`Booster` が入って**いて、コアの予測は `Booster` が担当

## よくあるハマりどころ
- ファイルがない／壊れてる → 読み込みでエラー
    - まず `latest_model_path.exists()` をチェック
- 特徴量の順番・本数が違う → 予測が変・エラー
    - **学習時と同じ並び**にそろえる
- 欠損値は `NaN` でOK
    - LightGBMは欠損を扱えます

## まとめ
- `lgb.Booster` = **LightGBMの予測エンジン本体**
- `model_file=...` で **学習済みモデルをロード**
- `predict` で結果が出せる