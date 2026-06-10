# 実装工程（04_implement）

本工程は、新規監視構築の設計書を作る工程ではなく、アセスメント結果から required_services と remediation_task を整理し、将来の是正実施に引き継ぐための Markdown 成果物を確定する工程である。

## 実装工程の目的

- assessment-sheet.md と gap-list.md の判定結果を是正候補に変換する
- 部分適合、非適合に対して完全適合に必要な対応内容を明文化する
- その対応が必要な理由と参考サイトを記録する
- 実際の情報取得、再確認は Azure CLI、Az PowerShell、REST API、KQL など非 GUI 手段で行う

## この工程で実施する内容

| 項目 | 内容 |
|------|------|
| 入力 | [requirements.md](../01_specify/requirements.md), [assessment-design.md](../02_plan/assessment-design.md), [task-list.md](../03_tasks/task-list.md), [assessment-sheet.md](../05_verify/assessment-sheet.md) |
| 処理 | 判定結果をもとに remediation_task、required_services、reason、reference_url を整理 |
| 出力 | [gap-list.md](../05_verify/gap-list.md) の充実化、[final-assessment-result.md](../output/final-assessment-result.md) への反映 |
| 禁止事項 | Azure Portal の手順書化、GUI 前提の操作説明、新規監視構築の実施記録 |

## 記載ルール

- 各不足項目には以下を必ず記載する
  - 完全適合にするために必要なこと
  - なぜ必要か
  - 参考サイト
  - 再確認に使う非 GUI の取得方法
- PowerPoint 化を想定し、1 項目 1 セクションで簡潔に記載する

## 実装工程の完了条件

- gap-list.md の全ての部分適合、非適合に remediation_task が設定されている
- remediation_task に reason と reference_url が付与されている
- required_services が [assessment-design.md](../02_plan/assessment-design.md) と整合している
- 再確認手段が Azure CLI、Az PowerShell、REST API、KQL のいずれかで記載されている

## 実装アプリ

- レポート生成スクリプト: [scripts/run_monitoring_assessment.py](scripts/run_monitoring_assessment.py)
- 実行ラッパー: [scripts/run_monitoring_assessment.ps1](scripts/run_monitoring_assessment.ps1)
- 依存ライブラリ: [requirements.txt](requirements.txt)
- 出力先: [../output/final-assessment-result.md](../output/final-assessment-result.md)

### 実行手順

```powershell
cd categories/01_監視_モニタリング/04_implement
../../.venv/Scripts/python.exe -m pip install -r requirements.txt
powershell -ExecutionPolicy Bypass -File ./scripts/run_monitoring_assessment.ps1
```

### 補足

- 本アプリは [../../Account/accounts.yml](../../Account/accounts.yml) の先頭スコープを対象として、Azure CLI の実行結果から最終レポートを再生成する。
- 旧実装フォルダ（alert-rules, kql-queries, runbooks, workbooks）はクリーンアップ済み。

## 変更履歴

| 日付 | 変更者 | 変更内容 |
|------|--------|---------|
| 2026-05-27 | 更新 | レポート生成アプリを配置し、旧フォルダをクリーンアップ |
| 2026-05-21 | 改訂 | 非 GUI のアセスメント結果整理工程に全面更新 |
