# 受入検証工程（05_verify）

本工程は、既存 Azure 環境から非 GUI 手段で取得した情報に基づくアセスメント結果が、自社監視基準と整合しているかを検証する工程である。

## 受入検証工程の目的

- 判定結果が requirements.md と assessment-design.md に沿っていることを確認する
- 判定根拠が Azure CLI、Az PowerShell、REST API、KQL で再取得できることを確認する
- Markdown 成果物が PowerPoint 化に適した粒度で整理されていることを確認する

## 管理対象ファイル

| ファイル | 用途 |
|------|------|
| [assessment-sheet.md](./assessment-sheet.md) | リソース単位の判定結果 |
| [gap-list.md](./gap-list.md) | 部分適合、非適合の差分管理 |
| [scenarios.md](./scenarios.md) | 判定再現性と受入シナリオ |
| [acceptance-criteria.md](./acceptance-criteria.md) | 受入基準 |
| [final-assessment-result.md](../output/final-assessment-result.md) | 最終報告（配布用） |

## 検証の基本方針

- Azure Portal の画面目視ではなく、非 GUI 手段で再取得できる証跡を受入根拠にする
- 各監視基準ごとに以下を確認する
  - 判定
  - 判定理由
  - 完全適合にするために必要なこと
  - なぜ必要か
  - 参考サイト
  - 証跡の取得コマンドまたは KQL
- PowerPoint 化を想定し、1 シナリオ 1 セクションで管理する

## 変更履歴

| 日付 | 変更者 | 変更内容 |
|------|--------|---------|
| 2026-05-21 | 改訂 | 非 GUI 証跡ベースの受入検証工程に全面更新 |
