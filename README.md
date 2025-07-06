# TIL リポジトリ
このリポジトリは、技術別に日々の学び（TIL: Today I Learned）を管理するためのものです。  

## ディレクトリ構成
```
TIL/
├── README.md                     # このファイル：リポジトリ概要と運用ルール
├── templates/                    # 新規 TIL 作成用のテンプレート
│   └── til-template.md           # 見出し、日付、サマリー、参考リンク欄などの雛形
├── assets/                       # 図やスクリーンショットなどの静的ファイル
│   └── images/                   # イメージファイル格納ディレクトリ
└── tech/                         # 技術カテゴリごとのディレクトリ
    ├── python/                   # Python 関連の学び
    │   ├── _README.md            # Python カテゴリの索引・概要
    │   ├── list-comprehension.md # TIL: リスト内包表記のまとめ
    │   └── argparse.md           # TIL: argparse の使い方
    ├── git/                      # Git 関連の学び
    │   ├── _README.md
    │   ├── issue-management.md   # TIL: GitHub Issue 管理のメリット
    │   └── branching-strategy.md # TIL: ブランチ戦略
    ├── docker/                   # Docker 関連の学び
    │   ├── _README.md
    │   └── cron-in-container.md  # TIL: コンテナ内 cron 設定
    └── …                         # 他の技術カテゴリも同様に追加
```

## 運用ルール
1. テンプレート利用
    - 新しい TIL エントリを作成する際は、`templates/til-template.md` をコピーし、内容を記入  
    - コピー先は `tech/<カテゴリ>/` 配下とし、ファイル名は `テーマ.md`（例: `cache-invalidation.md`） の形式  

2. カテゴリ索引 (`_README.md`) の更新
    - 各技術カテゴリの `_README.md` に、新規ファイルへのリンクを追加
    - リンク例: `- [2025-07-09] list-comprehension.md` のように日付やタイトルを併記しても良い

3. ファイルヘッダー（オプション）
    - Markdown の冒頭に以下のような YAML front-matter を書くと、日付やタグを明示できる
    ```yaml
    ---
    title: リスト内包表記
    date: 2025-07-09
    tags:
      - python
      - list-comprehension
    ---
    ```

4. 画像・図の管理
    - スクリーンショットや図は `assets/images/` に保存し、Markdown 内では相対パスで参照
    - 例: `![コンテナ内 cron 設定](../assets/images/docker-cron.png)`

5. コミットメッセージ
    - 新規 TIL 追加時は `docs: add TIL for <テーマ> in <カテゴリ>` のようにする

## メリット
技術別に情報をまとめることで、後から特定のトピックだけを振り返りやすい。  
テンプレートと索引を整備することで、情報の一貫性と回遊性が向上する。  
画像やリンクを体系的に管理できるため、ナレッジベースとして長期的に拡張可能です。  