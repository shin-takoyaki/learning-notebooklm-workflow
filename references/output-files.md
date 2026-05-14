# 出力ファイル

タイムスタンプ付きサブディレクトリが必要な場合を除き、すべての最終出力は `learning-output/` 配下に置きます。

## `learning-input.md`

含める内容:

- 学習テーマ
- 学習目的
- 学習の深さ
- 避けたい情報源
- 既存URL・資料・メモ
- NotebookLM自動化設定
- NotebookLMノート名

## `research-plan.md`

含める内容:

- 学習テーマ
- 学習ゴール
- 必須トピック
- 関連トピック
- 深掘りすべき論点
- 調査対象
- 優先情報源
- 避ける情報源
- NotebookLMに投入すべき資料候補
- クイズ生成に必要な知識粒度

## `sources.md`

各情報源について含める内容:

- タイトル
- URL
- 種別
- 信頼度
- 重要度
- 要約
- NotebookLMに入れるべきか
- クイズ生成に使えるか

## NotebookLMソース用Markdownファイル

### `00_overview.md`

- 学習テーマの全体像
- なぜ重要か
- 主要概念
- 学習順序
- 初学者が最初に理解すべきこと

### `01_core_concepts.md`

- 重要用語
- 定義
- 具体例
- 関連概念
- 誤解しやすい点

### `02_deep_dive.md`

- 仕組み
- 背景知識
- 実務での使われ方
- 比較
- 設計上の注意点
- よくある失敗

### `03_sources.md`

- 参照した情報源一覧
- 各ソースの要約
- 信頼度
- NotebookLM投入優先度

### `04_quiz_seed.md`

- クイズ化しやすい論点
- 引っかけ問題にできるポイント
- 実務シナリオ問題の材料
- 用語確認問題の材料
- 比較問題の材料

## `quality-harness-result.md`

`quality-harness.md` の形式を使います。

## `notebooklm-prompt.md`

ファイルのアップロード後にNotebookLMへ貼り付けられるプロンプトを作成します。

## `notebooklm-upload-plan.md`

含める内容:

- 連携方式: PlaywrightによるWeb UI自動化
- ノート名
- NotebookLM URL
- 使用するPlaywrightプロファイル
- 投入対象ファイル
- 追加対象URLソース
- 期待される完了条件
- 自動化失敗時の手動フォールバック

## `notebooklm-upload-result.md`

含める内容:

- 実行日時
- 成功/失敗/一部成功
- NotebookLMノートURL
- アップロード対象ファイル
- アップロード成功が確認できたファイル
- URLソース追加対象
- URLソース追加成功が確認できたURL
- URLソース追加に失敗したURL
- クイズ操作結果
- 手動介入した箇所
- 失敗理由
- 次にユーザーが行うこと

## `README.md`

含める内容:

- この学習パッケージの目的
- NotebookLMへの投入結果または投入手順
- 推奨投入ファイル
- NotebookLMで使うプロンプト
- クイズ生成の流れ
- 自動化できている部分
- 自動化失敗時に手動で行う部分
- 今後の自動化候補
