# 公開サーバー運用から学ぶ重要サイバー攻撃事例

この資料は、[検証・PoC向け公開サーバーセキュリティガイドライン](../PUBLIC_SERVER_SECURITY_GUIDELINE.md) に関連する過去の事例と、社内で発生経験のある重要なカテゴリを取り上げる。

公表資料を基に、複数の管理不備がどのように被害へつながったかを理解することを目的とする。攻撃方法の再現に必要な詳細は扱わない。

## 1. Equifax 情報漏えい（2017年）

事象の流れ:

1. 攻撃者がインターネット公開された消費者向けWebポータルを狙った
2. ポータルで使用されていた Apache Struts の既知の脆弱性に対するパッチが適用されていなかった
3. 侵入後、平文で保存された管理認証情報や不十分なネットワーク分離により、機密データへの到達範囲が広がった
4. 不審な通信の検知が遅れ、約1億4,700万人分の個人情報が影響を受けた

米国FTCは、脆弱性の通知後にパッチ適用指示が出ていたにもかかわらず、実施確認が不足していたと説明している。GAOは、資産の識別、検知、ネットワーク分離、データガバナンスを主要因として整理している。

公開サーバー運用への教訓:

- 脆弱性情報を受け取るだけでなく、対象資産の特定、適用、再スキャンまでを完了条件にする
- インターネット公開サーバーから機密データベースへ直接到達できないように分離する
- 管理認証情報を平文ファイルへ保存せず、シークレット管理機能を使用する
- TLS監視、アプリケーションログ、異常通信のアラートが実際に機能することを定期的に確認する

関連するガイドライン:

- [3. ネットワークとファイアウォール](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#3-ネットワークとファイアウォール)
- [5. OSとソフトウェア](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#5-osとソフトウェア)
- [6. アプリケーション、HTTPS、秘密情報](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#6-アプリケーションhttps秘密情報)
- [7. 最低限のログと確認](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#7-最低限のログと確認)

出典:

- [Federal Trade Commission: Equifax to Pay $575 Million as Part of Settlement](https://www.ftc.gov/news-events/news/press-releases/2019/07/equifax-pay-575-million-part-settlement-ftc-cfpb-states-related-2017-data-breach)
- [U.S. Government Accountability Office: Data Protection - Actions Taken by Equifax and Federal Agencies](https://www.gao.gov/products/gao-18-559)

## 2. Capital One クラウド情報漏えい（2019年）

事象の流れ:

1. 攻撃者がインターネット上のクラウド環境を探索し、公開アプリケーションを狙った
2. 米国司法省の公表資料によると、設定不備のあるWebアプリケーションファイアウォールを通じて侵入が行われた
3. 取得したクラウド認証情報に付与された権限を使い、アクセス可能なストレージからデータがコピーされた
4. 1億人を超える顧客・申込者の情報が影響を受け、外部からの通報を契機に調査と法執行機関への連絡が行われた

この事例は、WAFを配置していても、設定、クラウドIDの権限、ストレージへのアクセス制御が連鎖すると大きな被害につながることを示している。米国OCCは、クラウド移行前のリスク評価と、把握した不備の是正が不十分だったとして制裁金を科した。

公開サーバー運用への教訓:

- WAFを設置しただけで安全と判断せず、設定レビューと攻撃を想定したテストを行う
- サーバーやワークロードのクラウド権限を最小化し、不要なストレージを参照できないようにする
- SSRF対策やインスタンスメタデータへのアクセス制御を実施する
- 大量の一覧取得やデータ転送を検知し、クラウド監査ログをサーバー外で保全する
- クラウド事業者との責任分界を明確にし、自社側の設定を継続的に監査する

関連するガイドライン:

- [3. ネットワークとファイアウォール](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#3-ネットワークとファイアウォール)
- [6. アプリケーション、HTTPS、秘密情報](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#6-アプリケーションhttps秘密情報)
- [7. 最低限のログと確認](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#7-最低限のログと確認)

出典:

- [U.S. Department of Justice: Former Seattle tech worker convicted of wire fraud and computer intrusions](https://www.justice.gov/usao-wdwa/pr/former-seattle-tech-worker-convicted-wire-fraud-and-computer-intrusions)
- [U.S. Department of Justice: Seattle Tech Worker Arrested for Data Theft Involving Large Financial Services Company](https://www.justice.gov/usao-wdwa/pr/seattle-tech-worker-arrested-data-theft-involving-large-financial-services-company)
- [Office of the Comptroller of the Currency: OCC Assesses $80 Million Civil Money Penalty Against Capital One](https://www.occ.treas.gov/news-issuances/news-releases/2020/nr-occ-2020-101.html)

## 3. MiraiボットネットによるDynへのDDoS（2016年）

事象の流れ:

1. 初期設定の認証情報やハードコードされた認証情報を持つIoT機器がインターネット上で探索された
2. 多数のカメラやルーターなどがマルウェアに感染し、ボットネットへ組み込まれた
3. ボットネットからDNS事業者のDynへ大量の通信が集中した
4. DNSサービスが妨害され、依存していた多数のWebサイトが利用しにくい状態になった

Miraiは、1台ごとの弱い設定がインターネット規模で集約されると、第三者のサービスを停止させる攻撃基盤になることを示した。また、公開サービス側も大量通信をホスト単体で防ぐことは難しく、上流のDDoS対策が必要になる。

公開サーバー運用への教訓:

- 初期パスワードを直ちに変更し、不要な管理ポートとサービスを公開しない
- 外部公開資産を継続的に棚卸しし、意図しない待受ポートを検出する
- DDoS対策をホストファイアウォールだけに依存せず、CDN、クラウド事業者、レート制限を組み合わせる
- DNSを含む外部依存先を把握し、冗長化、監視、障害時の連絡経路を用意する
- 自組織の機器が攻撃に悪用されないよう、送信通信と異常なトラフィックも監視する

関連するガイドライン:

- [2. 最低限行う対策](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#2-最低限行う対策)
- [3. ネットワークとファイアウォール](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#3-ネットワークとファイアウォール)
- [4. SSHと管理者アカウント](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#4-sshと管理者アカウント)
- [7. 最低限のログと確認](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#7-最低限のログと確認)

出典:

- [NIST SP 1800-15B: Securing Small-Business and Home Internet of Things Devices](https://www.nccoe.nist.gov/sites/default/files/legacy-files/iot-ddos-nist-sp1800-15b-preliminary-draft-v2.pdf)
- [CISA: NSTAC Report to the President on Internet and Communications Resilience](https://www.cisa.gov/sites/default/files/publications/NSTAC%20Report%20to%20the%20President%20on%20ICR%20FINAL%20%2810-12-17%29%20%281%29-%20508%20compliant_0.pdf)

## 4. 公開サーバーの踏み台化（社内発生カテゴリ）

当社では、サーバーが第三者への攻撃に悪用される「踏み台化」に該当するカテゴリの事象が発生したことがある。本資料では個別案件の日時、システム、原因、影響は扱わず、検証・PoC用の公開サーバーで一般的に想定される流れとして整理する。

一般的に想定される事象の流れ:

1. 必要以上に公開されたポート、弱い認証、未修正の脆弱性などを通じてサーバーへ侵入される
2. 不正なプログラム、SSH公開鍵、定期実行設定などを設置され、攻撃者が継続利用できる状態になる
3. サーバーから外部へのスキャン、不正ログイン試行、DDoS、迷惑メール送信、暗号資産マイニングなどが行われる
4. 外部組織やクラウド事業者からの通報、通信量・負荷・利用料金の増加などを契機に発覚する

踏み台化されると、サーバー内に重要なデータがなくても、当社のIPアドレスから第三者へ攻撃が行われる。外部組織への被害、信用低下、クラウド利用料の増加に加え、サーバーに設定されていた認証情報を使って別環境へ侵入される可能性もある。

注意すべき兆候:

- 想定していない宛先への大量通信やポートスキャン
- CPU、メモリ、通信量、クラウド利用料金の急増
- 見覚えのないプロセス、コンテナ、ユーザー、SSH公開鍵
- 不審な `cron`、systemdサービス、起動スクリプト
- 身に覚えのないログインや認証失敗の増加
- クラウド事業者、通信事業者、外部組織からの不正利用通知

最低限の予防策:

- 公開するポートを必要最小限にし、SSHは接続元を制限して公開鍵認証を使用する
- 公開前にOSとソフトウェアを更新し、初期アカウントと初期パスワードを無効化する
- PoCサーバーへ本番環境の認証情報や、本番リソースへアクセスできるクラウド権限を設定しない
- 公開直後と設定変更後に、待受ポート、ログ、実行中プロセスを確認する
- 管理者と公開終了日を決め、検証終了後はサーバー、鍵、許可ルールを削除する

発覚時の初動:

1. サーバーを削除せず、セキュリティグループなどで外部通信から隔離する
2. 上長、管理者、セキュリティ担当へ直ちに報告する
3. ログ、クラウド監査記録、ディスクやスナップショットなどの証拠を保全する
4. 影響を受けた可能性のあるAPIキー、パスワード、SSH鍵を安全な端末から無効化・更新する
5. 外部への攻撃通信と影響範囲を確認し、クラウド事業者や関係先への連絡は管理者の指示に従う

独断で再起動、ログ削除、初期化を行うと、原因や影響範囲を確認するための証拠が失われる可能性がある。緊急停止が必要な場合も、可能な限り管理者またはセキュリティ担当の指示に従う。

関連するガイドライン:

- [2. 最低限行う対策](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#2-最低限行う対策)
- [3. ネットワークとファイアウォール](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#3-ネットワークとファイアウォール)
- [4. SSHと管理者アカウント](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#4-sshと管理者アカウント)
- [5. OSとソフトウェア](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#5-osとソフトウェア)
- [6. アプリケーション、HTTPS、秘密情報](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#6-アプリケーションhttps秘密情報)
- [7. 最低限のログと確認](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#7-最低限のログと確認)
- [9. 公開終了時の対応](../PUBLIC_SERVER_SECURITY_GUIDELINE.md#9-公開終了時の対応)

個別案件の詳細、調査記録、連絡先は、アクセス権を制限したインシデント管理資料で管理する。

## 5. 4事例に共通すること

| 共通点 | 必要な対策 |
| --- | --- |
| 公開資産や設定を正確に把握できていない | 管理者と公開期限の記録、公開ポートの確認 |
| 1つの不備から被害範囲が拡大した | ネットワーク分離、最小権限、シークレット管理 |
| 防御製品の存在だけでは被害を防げなかった | 設定確認、外部からの接続確認、ログ確認 |
| 検知や是正の遅れが被害を大きくした | 最低限のログ、報告先、公開終了日の管理 |
| 単一の防御層では限界があった | ホスト、クラウド、アプリケーション、上流サービスによる多層防御 |

検証・PoC用の公開サーバーであっても、ファイアウォールを有効にして終わりではない。公開範囲、更新、権限、ログを確認し、検証終了後に確実に削除する必要がある。
