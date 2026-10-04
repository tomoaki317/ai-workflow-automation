# AI Workflow Automation

n8n / Make / OpenAI API を使った業務自動化PoCです。

## Goal

問い合わせ内容をAIで分類・要約し、結果を保存・通知するワークフローを構築します。

## Planned Flow

Webhook
→ Input Validation
→ OpenAI
→ Structured Result
→ Conditional Routing
→ Google Sheets
→ Slack Notification

## Tech Stack

- n8n
- Make
- OpenAI API
- Docker
- Google Sheets
- Slack

## n8n Workflow

`n8n/inquiry-triage-workflow.public.json` is a sanitized workflow export for public use.

Before importing, configure your own credentials and resource IDs for:

- OpenAI
- Google Sheets
- Slack

## Status

Under development.
