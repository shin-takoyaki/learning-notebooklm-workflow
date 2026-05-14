---
name: learning-notebooklm-workflow
description: ユーザーが、テーマからインタビュー、調査計画、情報源収集、Markdown教材生成、品質評価、改善サイクルを経て、NotebookLMに投入できる学習パッケージを作成したい場合に使うスキルです。個人用Googleアカウント向けに、基本はPlaywrightでNotebookLMノート作成とソースアップロードを自動実行し、必要な場合のみ手動入力へフォールバックします。
metadata:
  short-description: NotebookLM学習パッケージを作成する
---

# NotebookLM学習ワークフロー

このスキルは、NotebookLMに読み込める学習教材を作成し、その後PlaywrightでWeb UIを操作してNotebookLMノートの作成と生成済みMarkdownソースのアップロードを自動実行するために使います。自動化が失敗した場合のみ、最終手段として手動入力・手動アップロードに切り替えます。

## 基本ルール

- スキルが選択された直後に調査を始めないでください。
- まずユーザーにヒアリングし、必須項目を集めます。
- 個人用Googleアカウントでは、NotebookLM Enterprise APIではなくPlaywrightによるWeb UI自動化を標準経路とします。
- 永続的な出力は、可能な限りMarkdownファイルとして残します。
- NotebookLM UI自動化は必ず試行します。ログイン、2要素認証、UI変更、アカウント制限で自動化が止まる場合は安全に停止し、`notebooklm-upload-result.md` に手動フォールバックを書きます。
- 手動入力・手動アップロードは、Playwright自動化が完了できない場合の最終手段として扱います。

## ヒアリング

ユーザーに入力を求めるときは、`references/interview-template.md` を読みます。

必須項目:

- 学習したいテーマ
- 学習目的
- 学習の深さ

任意項目:

- 避けたい情報源
- 既に持っているURL・資料・メモ
- NotebookLMノート名
- 使用するブラウザプロファイルまたはPlaywright専用プロファイル

3つの必須項目が揃ったら先へ進みます。NotebookLM自動化設定が不足している場合は、保守的なデフォルトを選びます。

- ノート名: 学習テーマ
- 自動化: PlaywrightでNotebookLMノート作成とソースアップロードを実行
- プロファイル: 現在のワークスペース配下の `.notebooklm-playwright-profile`

## 出力ディレクトリ

最終成果物は次の配下に作成します。

```text
learning-output/
```

`learning-output/` が既に存在し、ユーザーファイルを含む場合は、`learning-output/2026-05-08-テーマ名/` のようなタイムスタンプ付きサブディレクトリを使います。

## ワークフロー

1. ヒアリング内容から `learning-input.md` を作成します。
2. `research-plan.md` を作成します。
3. 情報源を収集・評価して `sources.md` にまとめます。
4. NotebookLM投入用のMarkdownファイルを生成します。
   - `00_overview.md`
   - `01_core_concepts.md`
   - `02_deep_dive.md`
   - `03_sources.md`
   - `04_quiz_seed.md`
5. 品質ハーネスを実行し、`quality-harness-result.md` を書きます。
6. 結果が `PASS` でない場合は、最大3サイクルまでパッケージを改善します。
7. `notebooklm-prompt.md` を作成します。
8. `notebooklm-upload-plan.md` を作成します。
9. PlaywrightでNotebookLMノート作成とソースアップロードを実行します。
   - NotebookLM投入用Markdownファイルをアップロードします。
   - `sources.md` に集約されたURLもNotebookLMのURL/ウェブサイトソースとして追加します。
10. `notebooklm-upload-result.md` を書きます。
11. `README.md` を作成します。
12. 最終的なパッケージ構成を検証します。

出力ファイルの要件は `references/output-files.md` を読みます。

## 情報源の優先順位

情報源は次の順に優先します。

1. 公式ドキュメント
2. 標準、RFC、仕様書、学術論文、一次情報
3. 信頼できる技術ブログ
4. GitHubリポジトリと実装例
5. 補助的な日本語記事

各情報源について、次を記録します。

- タイトル
- URL
- 情報源の種別
- 信頼度
- 重要度
- 要約
- NotebookLMに含めるべきか
- クイズ生成に使えるか

## 品質ハーネス

生成した教材を評価する前に、`references/quality-harness.md` を読みます。

判定値は次を使います。

- 合格（`PASS`）
- 追加調査が必要（`NEED_MORE_RESEARCH`）
- 書き直しが必要（`REWRITE_REQUIRED`）

判定が合格（`PASS`）でない場合は、不足点を特定し、追加調査を行い、Markdownファイルを更新して、ハーネスを再実行します。改善サイクルは3回で止め、残っている不足を明確に記録します。

## NotebookLM Playwright自動化

ブラウザ自動化を実行する前に、`references/playwright-notebooklm.md` を読みます。

Node.js と Playwright が利用できる場合は、Web UI自動化の実行に `scripts/notebooklm_web_upload.mjs` を使います。

Playwright自動化は標準経路です。ユーザーに「自動作成を試すか」を任意項目として選ばせず、必要な実行環境が揃っていれば自動でNotebookLMノート作成とソースアップロードを進めます。

スキルディレクトリから実行するデフォルトコマンド:

```bash
node scripts/notebooklm_web_upload.mjs --output-dir ../../../learning-output --notebook-title "テーマ" --browser-channel chrome
```

Playwright がインストールされていない場合は、黙ってスキップせず、必要なセットアップをユーザーに伝えます。

```bash
npm install -D playwright
npx playwright install chrome
```

自動化対象:

```text
https://notebooklm.google.com/
```

ユーザーのGoogleパスワードを尋ねてはいけません。必要な場合、Googleログインはユーザーがブラウザ上で完了する必要があります。

ログイン、2要素認証、UI変更、アカウント制限、Playwright未導入、通常Chrome/Edge未導入、Googleによる安全でないブラウザ判定などで自動化を完了できない場合は、`notebooklm-upload-result.md` に原因、手動アップロード手順、投入対象ファイル、可能ならNotebookLM URLを記録し、ユーザーが手動で入力・アップロードできる状態にします。

## NotebookLMプロンプト

`references/notebooklm-prompt-template.md` を使って `notebooklm-prompt.md` を作成します。

プロンプトでは、アップロード済みソースを使って次を生成するようNotebookLMに依頼します。

- 学習ロードマップ
- 概念説明
- 学習ガイド
- クイズ
- シナリオ問題
- 弱点復習計画
