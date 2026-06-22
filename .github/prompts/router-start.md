# SDD 開始プロンプト（ルーター判定用 / 対話型実行ガイド）

## 🚀 クイックスタート

### オプション 1: Copilot Chat で対話ヒアリング（推奨）

このチャットでそのままヒアリングを開始できます。PowerShell 実行は不要です。

実行方法:
1. このチャットで「ヒアリング開始」と入力する。
2. Copilot が 7 つの質問を順番に実施する（各質問で選択肢 + 自由入力）。
3. 回答内容からカテゴリを自動判定する。
4. 判定カテゴリの全工程（`01_specify` `02_plan` `03_tasks` `04_implement` `05_verify` `06_migration` `output`）配下ファイルを自動生成/更新する。

### オプション 2: 手動実行（プロンプト入力）

以下のプロンプトをそのままコピーして、Claude Opus 4.8 に入力してください。

---

あなたは SDD ルーターです。必ず `.github/agents/agents.md` に従って判定してください。

## 目的
新規依頼を 12カテゴリのいずれかに振り分け、最初に更新すべきファイルと次アクションを確定する。

## 入力（Claude Opus 4.8 でヒアリング）
以下の手順で、依頼者ヒアリングをサブエージェント呼び出しとして実施してから判定すること。

1. `Claude Opus 4.8` サブエージェントを呼び出し、必要情報を質問形式で収集する。
2. 各質問は「選択肢提示」と「自由入力」の両方をケースバイケースで実施する。
3. 回答不足がある場合は、同サブエージェントで追加質問を実施する。
4. 最終的に、下記の入力項目をすべて埋めてからルーター判定を行う。

補足ルール:
- 各質問は次の順で進める: 1) 選択肢から選んでもらう 2) 選んだ理由や詳細を自由入力してもらう。
- 選択肢に当てはまらない場合は「その他」を選択して自由入力を必須化する。

### サブエージェント呼び出しテンプレート
- subagent: Claude Opus 4.8
- mission: SDDカテゴリ判定に必要な要件ヒアリング
- questions:
	- 依頼種別:
		- 選択肢: 新規（新しい仕様を一から組み立てる） / 既存更新（展開済み内容への仕様追加または仕様変更）
		- 自由入力: 選んだ理由や対象の概要を記載
	- 依頼タイトル:
		- 自由入力: 依頼タイトルを具体名で記載
	- 依頼本文（実現したいこと）:
		- 自由入力: 実現したい内容を1〜3文で記載
	- 背景（なぜ必要か）:
		- 選択肢: 品質課題 / 工数削減 / 障害再発防止 / 監査対応 / コスト圧縮 / 顧客要望 / その他
		- 自由入力: 背景と発生している課題を記載
	- 期限/優先度:
		- 選択肢: 緊急（24時間以内） / 高（今週中） / 中（今月中） / 低（期限柔軟）
		- 自由入力: 具体期限と優先判断理由を記載
	- 制約（禁止事項・運用制約・影響許容範囲）:
		- 選択肢: 本番停止不可 / メンテ時間のみ可 / 追加コスト不可 / 人員固定 / セキュリティ基準厳守 / その他
		- 自由入力: 制約詳細と許容できる影響範囲を記載
	- 成果物の期待形（何を作るか）:
		- 選択肢: Web UI / バッチ・スクリプト / API・バックエンド / 運用手順書（Markdown） / 設計書・仕様書 / Excel・スプレッドシート / PowerPoint・報告資料 / 設定ファイル・IaC / テストコード / 一式対応 / その他
		- 自由入力: 必要な成果物と納品イメージを記載
	- 受入条件（テスト粒度）:
		- 選択肢: ユニットテスト（関数・モジュール単位） / 統合テスト（コンポーネント間連携） / E2Eテスト（画面・フロー全体） / 手動動作確認（目視・手順実行） / レビュー承認のみ / 監査証跡・エビデンス整備 / その他
		- 自由入力: 合格基準と確認方法を具体的に記載

### ヒアリング結果（今回の依頼）
- 依頼種別: {{新規 / 既存更新}}
- 依頼タイトル: {{依頼タイトル}}
- 依頼本文: {{依頼本文}}
- 背景: {{背景}}
- 期限/優先度: {{期限・優先度}}
- 制約: {{制約}}
- 成果物の期待形: {{成果物の期待形}}
- 受入条件: {{受入条件}}

## 実施ルール
1. まずカテゴリを 01〜12 から1つ選ぶ（複数候補がある場合は第1候補と第2候補を提示）。
2. 判定根拠をキーワードベースで明記する。
3. 更新対象は必ず `categories/<category>/` 配下を使う。
4. 初回から `01_specify` `02_plan` `03_tasks` `04_implement` `05_verify` `06_migration` `output` の全工程を更新対象に含める。
5. `01_specify` だけでなく `02_plan` `03_tasks` `04_implement` `05_verify` `06_migration` `output` も、依頼ごとの新規フォルダ（`<request-folder>`）配下にマークダウンを配置する。
   - フォルダ名は依頼タイトルを英数字ハイフン区切りへ正規化して作成する。
   - 例: `ai-ops-task-web-ui`
6. 次に `02_plan/<request-folder>/plan.md` へ進む条件を明記する。
7. 最後に sdd-quality-gate をどのタイミングで挿入するか明記する。
8. 各カテゴリに属さないスクリプトは `scripts/` 配下に新規作成して配置する。

## 12カテゴリ（判定先）
- 01: 監視_モニタリング
- 02: 運用補佐ツール開発_管理
- 03: インシデント_障害対応
- 04: 問い合わせ対応_サポート
- 05: 変更_リリース管理
- 06: 構成管理_資産管理
- 07: セキュリティ管理
- 08: バックアップ_リカバリ
- 09: キャパシティ管理
- 10: 権限管理
- 11: コスト管理
- 12: 統制管理

## 出力フォーマット（この形式を厳守）
### 1) ルーター判定
- 第1候補カテゴリ: <番号_カテゴリ名>
- 第2候補カテゴリ: <番号_カテゴリ名 or なし>
- 信頼度: <High/Medium/Low>
- 判定根拠キーワード: <3〜8個>

### 2) 初回更新対象
- requirements: categories/<category>/01_specify/<request-folder>/requirements.md
- plan: categories/<category>/02_plan/<request-folder>/plan.md
- tasks: categories/<category>/03_tasks/<request-folder>/tasks.md
- implement: categories/<category>/04_implement/<request-folder>/implement.md
- verify: categories/<category>/05_verify/<request-folder>/verification.md
- migration: categories/<category>/06_migration/<request-folder>/migration.md
- output: categories/<category>/output/<request-folder>/result.md
- common-scripts: scripts/<script-name>.(ps1|py|sh)

### 3) 今回の着手手順（最大7手順）
1. <手順>
2. <手順>
3. <手順>

### 4) Planへ進むゲート条件
- <条件1>
- <条件2>
- <条件3>

### 5) 品質ゲート挿入ポイント
- 要件品質ゲート: <いつ実施するか>
- Specify-Plan整合ゲート: <いつ実施するか>
- Verify証跡ゲート: <いつ実施するか>

### 6) 未確定事項（あれば）
- <確認質問1>
- <確認質問2>

---

---

## 📖 ヒアリング実行例

### 例 1: CPU アラート過検知（監視_モニタリング）

```
Q1. 依頼タイプを選択: 2 (既存改善)
依頼タイトル: CPU アラート過検知の是正

Q2. 実現したい分類: 1 (監視改善)
実現内容: 夜間に通知が多すぎるため、しきい値と通知条件を見直したい。

Q3. 背景の分類: 1 (品質課題)
背景: 当番負荷が高く、一次対応品質が落ちている。

Q4. 期限・優先度: 2 (高（今週中）)
具体期限: 今週中に完了したい。

Q5. 制約: 1 (本番停止不可)
制約詳細: 監視停止は禁止。本番影響を最小化すること。

Q6. 成果物: 2 (計画更新)
成果物内容: 新しいしきい値定義、通知ルール更新、段階的適用計画。

Q7. 受入条件: 2 (SLA遵守)
完了判定: 夜間アラート数を現在の 50% 以下に削減。

【判定結果】
第1候補: 01 - 監視_モニタリング (スコア: 8)
```

**自動生成されるファイル（全工程）:**
- `categories/01_監視_モニタリング/01_specify/cpu-alert-threshold-tuning/requirements.md`
- `categories/01_監視_モニタリング/02_plan/cpu-alert-threshold-tuning/plan.md`
- `categories/01_監視_モニタリング/03_tasks/cpu-alert-threshold-tuning/tasks.md`
- `categories/01_監視_モニタリング/04_implement/cpu-alert-threshold-tuning/implement.md`
- `categories/01_監視_モニタリング/05_verify/cpu-alert-threshold-tuning/verification.md`
- `categories/01_監視_モニタリング/06_migration/cpu-alert-threshold-tuning/migration.md`
- `categories/01_監視_モニタリング/output/cpu-alert-threshold-tuning/result.md`

### 例 2: 変更管理プロセス改善（変更_リリース管理）

```
Q1. 依頼タイプを選択: 1 (新規提案)
依頼タイトル: 変更申請から承認までのリードタイムを短縮

Q2. 実現したい分類: 5 (変更管理)
実現内容: 現在の変更申請フローが手動で 5〜7 営業日かかっているため、
         自動化と CAB プロセスの効率化で 2 営業日に短縮したい。

Q3. 背景の分類: 2 (工数削減)
背景: 運用チームが変更管理業務に月 40 時間を費やしており、本来の保守業務に支障。

Q4. 期限・優先度: 3 (中（今月中）)
具体期限: 今月末までに改善案を提出、来月から導入開始。

Q5. 制約: 6 (その他)
制約詳細: 追加コスト 50 万円以内。既存ツール（Jira、Slack）連携必須。

Q6. 成果物: 6 (一式対応)
成果物内容: プロセス設計、ツール構成案、実装手順、トレーニング資料。

Q7. 受入条件: 3 (手順完了)
完了判定: 変更申請から CAB 承認までが 2 営業日以内。月間工数を 40h → 15h に削減。

【判定結果】
第1候補: 05 - 変更_リリース管理 (スコア: 7)
第2候補: 02 - 運用補佐ツール開発_管理 (スコア: 5)
```

**自動生成されるファイル（全工程）:**
- `categories/05_変更_リリース管理/01_specify/change-approval-leadtime-reduction/requirements.md`
- `categories/05_変更_リリース管理/02_plan/change-approval-leadtime-reduction/plan.md`
- `categories/05_変更_リリース管理/03_tasks/change-approval-leadtime-reduction/tasks.md`
- `categories/05_変更_リリース管理/04_implement/change-approval-leadtime-reduction/implement.md`
- `categories/05_変更_リリース管理/05_verify/change-approval-leadtime-reduction/verification.md`
- `categories/05_変更_リリース管理/06_migration/change-approval-leadtime-reduction/migration.md`
- `categories/05_変更_リリース管理/output/change-approval-leadtime-reduction/result.md`

### 例 3: セキュリティ脆弱性対応（セキュリティ管理）

```
Q1. 依頼タイプを選択: 3 (不具合対応)
依頼タイトル: 定期セキュリティ監査による脆弱性の是正

Q2. 実現したい分類: 7 (セキュリティ対策)
実現内容: 外部セキュリティ監査で指摘された脆弱性 3 件に対する対策を実施したい。

Q3. 背景の分類: 4 (監査対応)
背景: 四半期ごとの外部監査に対応するため、発見された脆弱性を期限内に是正する必要がある。

Q4. 期限・優先度: 1 (緊急（24時間以内）)
具体期限: 明日までに対策計画を立案。来週中に実装完了。

Q5. 制約: 5 (セキュリティ基準厳守)
制約詳細: 当社のセキュリティポリシーに従うこと。外部ツールの導入は事前承認必須。

Q6. 成果物: 5 (検証記録)
成果物内容: 脆弱性対策記録、検証テスト結果、監査項目チェックリスト。

Q7. 受入条件: 5 (監査証跡整備)
完了判定: 監査で指摘された脆弱性がすべて Close。証跡が整備完了。

【判定結果】
第1候補: 07 - セキュリティ管理 (スコア: 9)
```

**自動生成されるファイル（全工程）:**
- `categories/07_セキュリティ管理/01_specify/security-vulnerability-remediation/requirements.md`
- `categories/07_セキュリティ管理/02_plan/security-vulnerability-remediation/plan.md`
- `categories/07_セキュリティ管理/03_tasks/security-vulnerability-remediation/tasks.md`
- `categories/07_セキュリティ管理/04_implement/security-vulnerability-remediation/implement.md`
- `categories/07_セキュリティ管理/05_verify/security-vulnerability-remediation/verification.md`
- `categories/07_セキュリティ管理/06_migration/security-vulnerability-remediation/migration.md`
- `categories/07_セキュリティ管理/output/security-vulnerability-remediation/result.md`

---

## 記入例（従来の手動方式・参考）
- 依頼タイトル: CPUアラート過検知の是正
- 依頼本文: 夜間に通知が多すぎるため、しきい値と通知条件を見直したい。
- 背景: 当番負荷が高く、一次対応品質が落ちている。
- 期限/優先度: 今週中 / 高
- 制約: 監視停止は禁止。本番影響を最小化すること。


