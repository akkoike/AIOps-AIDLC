# 展開・移行工程（06_migration）

本工程は、アセスメント結果を運用へ引き継ぎ、是正展開後に再アセスメントを実施して差分を追跡するための工程である。

## 展開・移行工程の目的

- [gap-list.md](../05_verify/gap-list.md) に基づく是正対応の展開管理
- 是正完了後の再アセスメント手順の標準化
- 非 GUI 手段で再取得できる運用証跡の継続管理

## 基本方針

- 運用引き継ぎ資料は Markdown で管理する
- 再アセスメントは Azure CLI、Az PowerShell、REST API、KQL を使って実施する
- Azure Portal の画面操作手順は本工程の正式手順に含めない

## 管理対象

| ファイル | 用途 |
|------|------|
| [handover.md](./handover.md) | 運用引き継ぎ、再アセスメント手順 |
| [gap-list.md](../05_verify/gap-list.md) | 是正対象と進捗管理 |
| [assessment-sheet.md](../05_verify/assessment-sheet.md) | 再評価の比較元 |
| [final-assessment-result.md](../output/final-assessment-result.md) | 最新の最終報告書 |

## 変更履歴

| 日付 | 変更者 | 変更内容 |
|------|--------|---------|
| 2026-05-21 | 改訂 | 非 GUI ベースの展開・再アセスメント工程に更新 |
