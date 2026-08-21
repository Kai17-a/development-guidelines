# OSSライセンス運用ガイド

このガイドは、OSSなどの採用から廃止までに必要な記録、確認、リリース作業を定める。利用可否の判断基準は[OSSライセンス利用ガイドライン](./OSS_LICENSE_GUIDELINE.md)、ライセンスごとの特徴は[OSSライセンスリファレンス](./OSS_LICENSE_REFERENCE.md)を参照する。

## 1. 採用申請

担当者は、直接依存、コピーしたコード、ビルド時の取得物について次を記録する。

| 項目 | 記録内容 |
| --- | --- |
| コンポーネント | パッケージ名、配布元、公式URL |
| 特定情報 | バージョン、コミット、ハッシュ、取得日 |
| ライセンス | SPDX ID、LICENSE・NOTICEの原文、例外、追加条件 |
| 利用方法 | 実行、改変、リンク、コピー、同梱、SaaSなど |
| 提供範囲 | 社内、顧客、一般配布、別法人、対象地域 |
| 改変 | 変更の有無、変更ファイル、パッチ保管先 |
| 依存関係 | 推移的依存、バンドル済みコード、ベースイメージ |
| 判断 | 許可、条件付き許可、法務確認、禁止、判断者、判断日 |

[ガイドラインの相談条件](./OSS_LICENSE_GUIDELINE.md#6-法務知財担当へ相談する条件)に該当する場合は、採用前に法務・知財担当へ相談する。

## 2. 開発中の管理

- ロックファイルを使用し、依存関係とバージョンを再現可能にする
- SCAなどで直接依存と推移的依存を継続的に検査する
- `LICENSE`、`COPYING`、`NOTICE`、ファイルヘッダー、パッケージメタデータを確認する
- GPL系の`-only`と`-or-later`、例外条項を区別し、SPDX式で記録する
- コピーしたソースと改変箇所を追跡し、上流のライセンス表示を削除しない
- バージョン更新時に、ライセンス、権利者、依存関係の変更を再審査する

リポジトリ画面に表示されるライセンス名だけで判断せず、取得したバージョンに含まれる原文を保管する。

## 3. リリース判定

リリース責任者は、最終成果物を基準に次を確認し、証跡を保管する。

- [ ] SBOMまたは依存関係一覧を生成した
- [ ] 未特定ライセンス、禁止ライセンス、未承認コンポーネントがない
- [ ] `THIRD_PARTY_NOTICES`などに必要な著作権表示、ライセンス文、NOTICEを収録した
- [ ] コピーレフト対象、改変箇所、提供すべき対応ソースを特定した
- [ ] ソース提供方法、提供期間、問い合わせ窓口を決めた
- [ ] インストーラー、コンテナ、機器、ドキュメント、フォント、Web資産も検査した
- [ ] 製品EULA、顧客契約、アプリストア規約がOSSの権利を制限していない
- [ ] 承認記録、SBOM、スキャン結果、配布した表示とソースの写しを保存した

## 4. 第三者ライセンス表示

実際のライセンス条件が要求する全文と形式を優先する。管理用の最小例を次に示す。

```text
Third-Party Software Notices

Component: <name>
Version: <version or commit>
Source: <official source URL>
Copyright: <copyright notice from upstream>
License: <SPDX identifier>
Modifications: <none / summary and date>

<required license text and applicable NOTICE content>
```

対応ソースを提供する場合は、対象製品とバージョン、取得方法、提供期限、連絡先を明記する。Webで提供する場合も、リンク切れや認証期限によって取得不能にならないよう管理する。

## 5. 運用と廃止

- 脆弱性と同様に、ライセンス変更や違反通知を追跡する
- 削除した依存関係も、過去リリースの証跡と対応ソースを社内保存基準に従って保持する
- 違反の疑いを発見した場合は、証拠を保全し、配布拡大を止め、法務・知財担当と是正する
- 違反通知に対して独断で権利者へ回答しない

## 6. 部署内の管理ルール

- ライセンス表記は可能な限りSPDX IDで統一し、推測値と確認済み値を区別する
- 許可リストはライセンス単体ではなく、利用形態と義務を組み合わせて定義する
- 禁止・要審査ルールは、開発端末、CI、リポジトリ、リリース工程で同じ基準を使う
- SBOMはSPDXまたはCycloneDXなどの機械可読形式とし、製品とバージョンごとに追跡する
- 自動スキャン結果は人がレビューし、誤検知、例外、デュアルライセンス、コード同梱の有無を確認する
- 教育、役割分担、申請窓口、違反時の是正手順を文書化する

## 7. 参考資料

- [OpenChain: ISO/IEC 5230 License Compliance](https://openchainproject.org/license-compliance)
- [NTIA: The Minimum Elements for an SBOM](https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom)
- [SPDX: Handling License Info](https://spdx.dev/learn/handling-license-info/)
- [GitHub Docs: Licensing a repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository)
