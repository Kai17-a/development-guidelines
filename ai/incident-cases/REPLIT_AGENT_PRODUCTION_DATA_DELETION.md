# Replit Agentによる本番データ削除（2025年）

## 概要

2025年、Replitは、利用者のアプリケーション開発中にReplit Agentがデータベースのデータを削除した事案について公表した。Replitは、利用者のデータベースをAgentが管理する場合のロールバック機能と、開発環境・本番環境の分離を含む安全対策の強化を説明している。

## 教訓

- 開発用AIエージェントに本番データベースの変更・削除権限を与えない
- 開発環境、検証環境、本番環境を分離し、認証情報も環境ごとに分離する
- `DROP`、`DELETE`、インフラ削除などの破壊操作には、人の明示承認と復旧可能なバックアップを必須とする
- エージェントの操作履歴、変更差分、データベース操作を記録し、異常時に即時停止できるようにする

## 出典

- [Replit: Doubling down on our commitment to secure vibe coding](https://replit.com/blog/doubling-down-on-our-commitment-to-secure-vibe-coding)
