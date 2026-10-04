# AI Workflow Automation

問い合わせ内容をAIで分類・要約し、優先度に応じて記録・通知するn8n PoCです。

## Workflow

```text
Webhook
→ Input Validation
→ OpenAI
→ JSON Parse / Validation
→ Manual Review Routing
→ Google Sheets
→ Priority Check
→ Slack Notification
```

必須入力（`inquiry_id` と `message`）を確認し、OpenAIで問い合わせを分類・要約します。AI出力をJSONとして解析・検証した後、結果をGoogle Sheetsに保存します。`high` の場合だけSlackへ通知し、`medium` と `low` は通知しません。入力不正、AI出力不正はそれぞれ専用のWebhookレスポンスを返します。

## Implemented Features

- Webhookによる問い合わせ受付
- `inquiry_id` と `message` の必須入力チェック
- OpenAIによるカテゴリ分類・優先度判定・要約
- AI出力のJSONパース
- `category` / `priority` の許可値検証
- 不正なAI出力を `manual_review` に分岐
- Google Sheetsへの分析結果保存
- `high` priority のみSlack通知
- 正常系 / 入力不正 / AI出力不正の分岐とWebhookレスポンス

## Safety Design

- AI出力をそのまま後続処理へ渡さず、JSONとして解析可能か確認
- `category` / `priority` が許可値に含まれるか検証
- `summary` / `required_action` の必須項目を確認
- 不正なAI出力は `manual_review` に分岐
- 認証情報はn8n Credentialsで管理し、GitHubへ含めない

## Test Cases

### High Priority

- 複数ユーザーに影響
- 代替手段なし
- 業務停止
- Google Sheetsへ保存
- Slack通知あり
- `notification_sent = true`

### Medium Priority

- 一部機能に問題
- 代替手段あり
- 業務継続可能
- Google Sheetsへ保存
- Slack通知なし
- `notification_sent = false`

## Tech Stack

- n8n
- OpenAI API
- Docker
- Google Sheets
- Slack

## Setup

1. n8nを起動します（Docker Composeを利用できます）。
2. n8nにOpenAI、Google Sheets、SlackのCredentialsを登録します。
3. `n8n/inquiry-triage-workflow.public.json` をインポートします。
4. Google SheetsのファイルとSlackチャンネルを自分の環境に合わせて設定し、Webhookを有効化します。

## n8n Workflow

`n8n/inquiry-triage-workflow.public.json` is a sanitized workflow export for public use.

Before importing, configure your own credentials and resource IDs for:

- OpenAI
- Google Sheets
- Slack

## Status

PoC implemented.
