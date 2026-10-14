# 通知デザイン更新

`bot.py` と `.github/workflows/patch.yml`、`.github/workflows/discord-test.yml` をリポジトリ直下の対応パスに上書きしてください。
`data/state.json` は**既存の履歴を保持**するため、絶対に上書きしないでください。

変更点: Geminiのカテゴリ別JSON出力、パッチ11カテゴリ・スキン6カテゴリ、空カテゴリ非表示、Discordフィールドおよびメッセージ文字数制限に応じた分割。

Secrets: GEMINI_API_KEY / DISCORD_WEBHOOK_URL。要約APIのモデル設定: gemini-3.5-flash-lite。

検証: `.github/workflows/discord-test.yml` を手動実行すると既存13.08を再投稿します。`state.json` は変更しません。

注意: スキン検知対象は従来どおり公式ゲームアップデート一覧のタイトル検索です。Discord投稿途中の失敗後に再試行すると重複する可能性があります。AI要約の内容は公式原文との照合が必要です。
