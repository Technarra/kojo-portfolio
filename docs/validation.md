# コウジョー — 検証結果

[READMEへ戻る](../README.md)

## 基準と確認日

2026-10-05に、iOS・Android・API・契約を統合した同一commit **`e5f4e8cfb6dcbacd2556aeb622f52bc7af059c34`** のCIが成功で終了したことを確認しました。以下はその結果の要約で、2026-10-06の資料編集でアプリ試験を新たに実行した結果ではありません。

| 検証 | 対象 | 結果 |
| --- | --- | --- |
| 資料検証 | 原本・生成本文・合成評価データ等の整合 | 成功 |
| API単体 | ロジック・契約・失敗系 | 成功 |
| PostgreSQL 17 | 移行・複数sessionのqueue等の並行処理 | 成功 |
| API契約 | OpenAPIの検査 | 成功 |
| Compose | API・worker・DBの起動とhealth | 成功 |
| iOS simulator | build・XCTest、実Keychainを使う合成試験 | 成功 |
| Android | debug build・JVM単体試験 | 成功 |

## 確認したCI記録

- [資料：37267005178](https://github.com/Technarra/kojo/actions/runs/37267005178)
- [API・PostgreSQL・契約・Compose：37267005068](https://github.com/Technarra/kojo/actions/runs/37267005068)
- [iOS・Android：37267005146](https://github.com/Technarra/kojo/actions/runs/37267005146)

記録は非公開本体repoにあり、一般閲覧できるリンクではありません。この公開資料では確認済みの範囲・対象commit・結果を説明し、内部ログや本体の履歴は公開していません。

## 統合時の問題と修正

| 実際の問題 | 修正 | 再確認 |
| --- | --- | --- |
| Composeが合成評価ファイルをコピーできない | Docker除外設定へ必要な2ファイルだけを許可 | Compose起動成功 |
| Android SDKコマンドがPATHにない | runnerの既存SDKの配置に合わせる | Android build・単体成功 |
| iOSのOpenAPI生成pluginが未信頼 | 固定されたApple packageのURL/revisionを検査し、該当pluginだけを信頼 | build成功 |
| 未署名simulatorでKeychain試験が失敗する | simulatorをadhoc署名。試験・権限判定を弱めない | iOS XCTest成功 |

本番の秘密情報・開発者証明書は使っていません。

## この結果に含まれないもの

- 端末とAPIを接続した一巡E2E、実機の撮影・書き出し・再生。
- 実工場の教材・質問、外部AIの意味の忠実性、実APNs/FCM配信。
- 保存容量、バックアップ、保持・削除、実運用の認証、本番受入。

30件の合成ケースやfake providerでの検証を、実AIの正答率や工場の安全性・導入効果へ読み替えていません。
