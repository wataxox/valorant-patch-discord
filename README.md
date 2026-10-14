# VALORANT Patch → Discord

VALORANT公式日本語パッチノートおよび新スキン紹介・トレーラー記事を監視し、新規記事をGeminiで要約してDiscordに自動投稿します。

## 初期設定

1. GitHubで**公開リポジトリ**を作り、このフォルダの全ファイルをpushする。
2. Discord: 対象チャンネルの「チャンネルの編集 → 連携サービス → ウェブフック」で新規Webhookを作成し、URLをコピーする。
3. Google AI Studio https://aistudio.google.com/apikey でAPIキーを作成。無料枠利用可能なモデルと課金設定を確認する。
4. GitHub: Settings → Secrets and variables → Actions → New repository secret に `DISCORD_WEBHOOK_URL` と `GEMINI_API_KEY` を登録する。
5. GitHub: Settings → Actions → General → Workflow permissions で GitHub Actionsによる書き込みを許可する。
6. Actions → VALORANT Patch and Skin Notifications → Run workflow を実行する。初回は記事を投稿せず既存記事を既読化する。
7. 以降は6時間ごとに新着確認。記事が見つかればDiscordに投稿する。

## いま公開済みの最新記事を試験投稿したいとき

一度初回実行して既読状態にした後、テスト専用のチャンネルにWebhookを切り替え、`data/state.json` の最新記事URLを `seen` から1件だけ削除してcommitし、Actionsを手動実行してください。実行後は自動的に再び記録されます。実運用チャンネルでの重複投稿に注意。

## 環境変数

- `GEMINI_API_KEY` (必須)
- `DISCORD_WEBHOOK_URL` (必須)
- `GEMINI_MODEL` (任意、デフォルト `gemini-2.5-flash-lite`)
- `POST_ON_FIRST_RUN` (デフォルト false。trueの場合、状態空なら最大3件を投稿)

## 運用上の注意

- GitHub Actionsの実行時間は正確とは限らず、公開リポジトリのscheduleは長期間活動なしで停止することがあります。
- Gemini無料枠はモデル・アカウント・時期に依存します。無料枠が使えるかGoogle AI Studioの画面で確認してください。
- GitHubへの状態commitが失敗すると投稿済み記事が次回再投稿され得ます。完全なexactly-once配信ではありません。
- 通知が遅延した場合、一度に最大3件投稿します。最古の未処理記事から送信します。
- 公式サイトのHTML変更時には、`discover()` / `extract_article()` の修正が必要です。
- 公開リポジトリには秘密情報を書かないでください。Webhook URL・APIキーはSecretsのみで保管してください。
- 公式パッチ一覧で過去記事が掲載範囲から消えると、その記事は検知できません。

## スキン情報の通知

- 監視対象: https://playvalorant.com/ja-jp/news/game-updates/ に掲載される新スキン紹介・トレーラー記事。
- パッチとは別に既読管理し、既存リポジトリへの導入初回は過去のスキン記事を通知しません。
- 価格・発売日・収録武器は公式本文に明記されたものだけ要約します。
- スキンの販売開始そのものや日替わりストア、ナイトマーケットの個別商品は対象外です。
- タイトルやリンクが公式サイトで変わると検出条件の調整が必要です。
