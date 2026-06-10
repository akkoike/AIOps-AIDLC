# 01 - 監視（モニタリング）

> 既存 Azure 環境の監視実装状況を、非 GUI の取得手段で棚卸しし、自社監視基準に対する適合性を評価して Markdown に記録する

## 業務概要

本カテゴリは、既存 Azure 環境に対して Azure Resource Graph、Azure CLI、Az PowerShell、REST API、Log Analytics KQL などの非 GUI 手段で構成情報と監視証跡を取得し、自社監視基準 M-01〜M-11 に対する適合性をアセスメントするための SDD 一式である。最終成果物は Markdown で管理し、Microsoft Copilot による PowerPoint 化を前提としたシナリオ構成で整理する。

## このカテゴリで実施すること

- 対象サブスクリプション、リソースグループ、リソース一覧を非 GUI で取得する
- 既存の Azure ネイティブ監視実装を証跡ベースで確認する
- 各リソースに対して M-01〜M-11 の適用可否と判定結果を記録する
- 部分適合、非適合について完全適合に必要な対応、理由、参考サイトを記録する
- assessment-sheet.md と gap-list.md を中核成果物として更新する

## 情報取得手段

| 手段 | 主用途 |
|------|--------|
| Azure Resource Graph | 対象リソース棚卸し、拡張機能、診断設定、関連付け確認 |
| Azure CLI / Az PowerShell | 監視設定、診断設定、DCR、Action Group、Health 情報取得 |
| Azure REST API | CLI で不足する情報の補完取得 |
| Log Analytics KQL | Perf、Heartbeat、Event、Syslog、AzureActivity 等の証跡取得 |
| ARM Export / 既存テンプレート | 既存構成の補助確認 |

## 成果物の基本ルール

- 最終成果物は Markdown とする
- 各判定には「適合 / 部分適合 / 非適合 / N/A」を必ず記載する
- 部分適合、非適合には「完全適合にするために必要なこと」「なぜ必要か」「参考サイト」を必ず記載する
- Microsoft Copilot で PowerPoint 化しやすいよう、1 観点 1 セクションのシナリオ構成で記述する

## SDD 管理構成

### ディレクトリ構成

```
01_監視_モニタリング/
├── README.md
├── 01_specify/
│   └── requirements.md
├── 02_plan/
│   ├── adr-summary.md
│   ├── architecture.md
│   ├── assessment-design.md
│   └── tool-selection.md
├── 03_tasks/
│   └── task-list.md
├── 04_implement/
│   └── README.md
├── 05_verify/
│   ├── README.md
│   ├── acceptance-criteria.md
│   ├── assessment-sheet.md
│   ├── gap-list.md
│   └── scenarios.md
└── 06_migration/
    ├── README.md
    └── handover.md
└── output/
    └── final-assessment-result.md
```

### SDD 7工程の適用

#### 1. 原則決定
- [Constitution.md](../../Constitution.md) に準拠する

#### 2. 企画・要件定義
- 自社監視基準と判定ルールを定義する
- 非 GUI 手段で取得する証跡の種類を定義する
- Markdown 出力と PowerPoint 化を前提とした記載ルールを定義する

#### 3. 設計計画
- Azure ネイティブ機能を使った判定方法を設計する
- Resource Graph、CLI、REST、KQL の役割分担を定義する
- 不足時の required_services と参考情報の出し方を定義する

#### 4. タスク分割
- 棚卸し、判定、証跡記録、差分整理、成果物更新の順でタスク化する
- 各タスクは assessment-sheet.md と gap-list.md に直結する単位で管理する

#### 5. 実装
- 本カテゴリでは新規構築ではなく、是正案と required_services の整理を行う
- 実装工程の成果物は是正案の Markdown 化であり、GUI 操作手順書は作成しない

#### 6. 検証・受入
- 判定根拠が非 GUI 手段で再取得できることを確認する
- 仕様、設計、タスク、成果物の整合を確認する

#### 7. 展開・運用
- gap-list.md をもとに是正展開と再アセスメントを管理する
- 定期再評価と差分確認を運用サイクルに組み込む

## 変更履歴

| 日付 | 変更者 | 変更内容 |
|------|--------|---------|
| 2026-04-18 | 初版作成 | カテゴリ定義・SDD管理構成の策定 |
| 2026-05-21 | 改訂 | 非 GUI の Azure 情報取得によるアセスメント型運用に全面更新 |
