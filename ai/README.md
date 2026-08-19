# AI利用ガイド

このディレクトリには、生成AIを業務で安全かつ効果的に利用するためのガイドラインと、再発防止に役立てるインシデント事例を置く。

## ガイドライン

- [AI利用ガイドライン](./AI_USAGE_GUIDELINE.md): 対象者、利用条件、入力・出力の取り扱い、運用体制を定める全社共通ルール
- [開発におけるAIエージェント利用ガイドライン](./AI_AGENT_DEVELOPMENT_GUIDELINE.md): AIエージェントによる開発作業の権限、レビュー、検証、運用ルール

## インシデント事例一覧

| 区分 | 事例 | 主な教訓 |
| --- | --- | --- |
| 国内 | [NICTの音声コーパス誤公開（2026年）](./incident-cases/NICT_AUDIO_CORPUS_DISCLOSURE.md) | 公開前確認、アクセス管理、配布先の追跡 |
| 海外 | [Samsung Electronicsでの機密情報入力（2023年）](./incident-cases/SAMSUNG_CONFIDENTIAL_DATA_INPUT.md) | 情報分類、承認済み環境、入力制御 |
| 海外 | [ChatGPT障害による情報露出（2023年）](./incident-cases/CHATGPT_OUTAGE_DATA_EXPOSURE.md) | 委託先リスク評価、最小限入力、通知・初動 |
| 海外 | [Air Canadaのチャットボット誤案内（2024年）](./incident-cases/AIR_CANADA_CHATBOT_MISINFORMATION.md) | 正確性試験、有人対応、回答の監視 |
| 海外 | [Mata v. Aviancaの架空判例引用（2023年）](./incident-cases/MATA_V_AVIANCA_FALSE_CITATIONS.md) | 一次情報による出典確認、専門家レビュー |
| 海外 | [イタリア当局によるChatGPTへの措置（2023年・2024年）](./incident-cases/ITALY_GARANTE_CHATGPT_ACTION.md) | 法的根拠、透明性、年齢確認、漏えい通知 |
| 海外・開発AIエージェント | [Replit Agentによる本番データ削除（2025年）](./incident-cases/REPLIT_AGENT_PRODUCTION_DATA_DELETION.md) | 開発・本番の分離、破壊操作の承認、復旧手段 |
| 海外・開発AIエージェント | [Amazon Q Developer拡張のサプライチェーン侵害（2025年）](./incident-cases/AMAZON_Q_DEVELOPER_EXTENSION_COMPROMISE.md) | トークンの最小権限、リリース保護、拡張機能の更新 |
