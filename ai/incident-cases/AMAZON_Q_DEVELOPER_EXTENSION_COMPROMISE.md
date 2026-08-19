# Amazon Q Developer拡張のサプライチェーン侵害（2025年）

## 概要

2025年、Amazon Q DeveloperのVisual Studio Code拡張で、過度な権限を持つGitHubトークンがCodeBuild設定に含まれていたため、脅威アクターが拡張のオープンソースリポジトリへ悪意あるコードをコミットできた。AWSは、そのコードがリリースに自動的に含まれたと公表している。

## 対応

AWSは、認証情報を失効・交換し、悪意あるコードを削除したうえで、修正版のバージョン1.85.0をリリースした。この事案はCVE-2025-8217として扱われている。

## 教訓

- 開発・ビルド用トークンは、リポジトリや必要な操作だけに権限を限定する
- 拡張機能・依存関係・自動リリース経路への変更は、レビュー、保護ブランチ、署名・来歴確認などで保護する
- AIエージェントがシェルやクラウド操作を行える場合、拡張機能・ツールチェーンの侵害を前提に最小権限と実行承認を適用する
- 利用中のAIエージェントと拡張機能のセキュリティ通知を監視し、修正版へ速やかに更新する

## 出典

- [AWS: Security Update for Amazon Q Developer Extension for Visual Studio Code (Version #1.84)](https://aws.amazon.com/security/security-bulletins/AWS-2025-015/)
