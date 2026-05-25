# NV開発AIワークベンチ — 作業ログ

作成: 2026-05-24  
対象ファイル: `nv_ai_workbench/nv_workbench_mockup.html`

---

## やったこと

### 2026-05-24 セッション1：ワークベンチ構築

**ベースファイル群の作成（22ファイル）**

| フォルダ | 内容 |
|---------|------|
| `common/` | nv_phenomenon_catalog.csv / vehicle_system_catalog.csv / analysis_method_catalog.csv / judgment_rule.yaml / data_schema.md |
| `scenarios/scenario_001_premium_ev_crossover/` | 01_strategy / 02_requirement / 03_system / 04_evaluation の各データファイル |
| `workbench/` | run_workflow.md / prompt_templates/ / execution_log.md |
| ルート | README.md / AGENT.md / ROOTMAP.md |

元資料: `NV開発AIワークベンチ_作成手順書.docx`（PowerShell WindowsBase経由で内容抽出）

---

### 2026-05-24 セッション2：HTML mockup v0.1 作成

**構成：**
- 3パネルレイアウト：チャット(320px) | サイドバーナビ(180px) | コンテンツ
- コンテンツセクション：dashboard / flow / strategy / requirement / eval / ranking / trace / evidence / decision
- 30秒デモスクリプト（17ステップ）
- QAチップ（8種）

---

### 2026-05-24 セッション3：HTML mockup v0.2 — 4点改善

**改善内容：**

1. **開発工程タイムライン追加**
   - ヘッダー下部に常時表示
   - 商品企画✓ → 先行開発✓ → 要求定義✓ → **P0試作評価★（現在地）** → P1試作評価 → 量産DR → 量産

2. **思考プロセス表示**
   - `Real AI` モード時に各ステップの思考ログをアニメーション表示
   - 📂読み込み → 🔍照合 → ⚖️スコアリング → ✅結論 の流れ
   - クリックで折りたたみ可能

3. **ゲージバー可視化**
   - ダッシュボード：RSLT01-06 の実測値 vs 目標ラインバー（赤/緑色分け）
   - 課題ランキング：ISS01-05 のスコアメーター

4. **Demo / Real AI 切り替え**
   - デモモード：高速スクリプト再生（約30秒）
   - Real AI モード：ローディング→思考プロセス→回答の流れ

**問題点（次セッションで対処）：**
- トークン上限(32,000)を超えたため、一括書き換えを断念 → セクション別Edit方式に変更
- 初期メッセージのトーンが不自然（「NVから始めず」等、誰も聞いていない前置き）
- デモシナリオが荒く、見る側に何が起きているか伝わりにくい
- ステップごとの「状況設定」が欠如（なぜ今このことを議論しているのかが不明）

---

## データ連動の状況

### HTML ↔ 実ファイルの対応

| HTMLの表示 | 参照元ファイル | 一致状況 |
|-----------|--------------|---------|
| 市場トレンドTR01〜TR06 | `01_strategy/market_trend.yaml` | ✅ 一致（内容を読んで反映） |
| 商品性価値V01〜V05 | `01_strategy/product_concept.yaml` | ✅ 一致 |
| 設計変更DC01〜DC05 | `03_system/design_change.csv` | ✅ 一致 |
| 測定結果RSLT01〜13 | `04_evaluation/measurement_result.csv` | ✅ 一致（数値完全一致） |
| 官能コメントCMT01〜CMT05 | `04_evaluation/subjective_comment.md` | ✅ 一致 |

**重要な注意点：**  
現在のHTMLはブラウザ上で動作するため、ローカルファイルをJSで直接読むことはできない（ブラウザのセキュリティ制約）。  
→ 現状：Claudeが実ファイルを読んでデータを確認し、そのまま HTMLにハードコード  
→ 本番実装案：Node.jsサーバー or バックエンドAPI経由でファイルを読み込む形に移行

---

---

## 2026-05-24 セッション4：シナリオ設計 v2（相談内容の記録）

### ユーザーフィードバック
- 初期メッセージが身構えた言葉から始まっている（「NVから始めず〜」）→ 誰も聞いていない
- シナリオが荒く、見る側に何が起きているかが伝わらない
- STEPは一気通貫ではなく、各Stepに判断ポイントがあり、そこで対話できる設計にする

### デモ対象
- NV開発エンジニア ＋ AI活用推進担当者

### 見せたいAIらしさ（4点すべて）
1. 複数ファイルを横断して自動でつなぐ
2. 測定値と官能コメントを自動で照合・解釈する
3. 判断根拠をトレースとして自動記録する
4. 商品性価値から逆算して課題の優先順位を決める

### 新シナリオ設計（Step別）

| Step | 状況設定 | AIが見せること | 判断ポイント |
|------|---------|--------------|------------|
| **Step 1** キックオフ・市場分析 | 新型EV Crossoverの開発がキックオフした直後。まず外部環境の整理から | 競合・市場トレンド（TR01〜TR06）を整理。「どこで差別化できるか」を提示 | **「この車は何で勝負するか」** |
| **Step 2** 商品コンセプト提案 | 市場を踏まえてコンセプトの方向性を複数案で提示 | A:後席快適性特化 / B:EV静粛性重視 / C:走行性能との両立 — 推奨案と根拠を提示 | **「どのコンセプトで進めるか」** |
| **Step 3** NV要求の導出 | コンセプト確定後、顧客体験シーン→NV要求に落とし込む | V01/V02→シーン→NV現象→目標値の一本線。要求の優先順位付け | **「どのNV要求を最優先にするか」** |
| **Step 4** P0試作評価レビュー | P0試作が上がった。設計変更5件を踏まえて評価結果を読む | 測定値+官能コメントを自動照合。「数字の裏にある体験」を言語化 | **「何が問題か、なぜ問題か」** |
| **Step 5** 課題ランキングと根拠 | 課題が出た。どれを優先するか判断が必要 | 6軸スコアリングで優先順位付け。「ロードノイズよりこもりが1位の理由」を説明 | **「優先順位の根拠を説明できるか」** |
| **Step 6** DR向け判断整理 | 来週DRがある。方針と根拠をまとめる必要がある | DRBFM・判断メモ・知見カードを自動生成。タイヤ再選定とブッシュ剛性最適化を方針として提示 | **「DRで何をどう説明するか」** |

### UI設計の変更方針
- Step を一気通貫の自動再生ではなく、**Stepごとの独立パネル**にする
- 各Stepに「状況設定テキスト（文脈）」を明示
- Stepに入るたびにAIが状況を整理して口火を切る
- ユーザーはそのStepの範囲内で自由に質問できる（Step固有のクイックチップ）
- 「次のStepへ →」ボタンで進む

## 2026-05-24 セッション5：HTML v0.3 — 6ステップ再構築

### 実施内容

1. **Word要求書生成**  
   `D:\temp\make_docx.py` を実行し `NV_workbench_data_requirements.docx` を生成（ユーザー提供のサンプルデータ作成依頼用）

2. **HTML v0.3 — 新6ステップ構造への全面改修**

   **新セクション構成：**
   | ID | 内容 |
   |----|------|
   | `step1` | 市場を読む（ベンチマークスパイダーチャート + 競合比較テーブル + 市場トレンド） |
   | `step2` | コンセプトを決める（A/B/C案比較カード + 商品性価値確定） |
   | `step3` | 前機種との違いを予測する（設計変更リスクマトリクス + アーキテクチャースパイダーチャート + NV要求マップ） |
   | `step4` | 試作評価結果を読む（旧eval → ゲージバー + 官能照合） |
   | `step5` | 課題の優先順位を決める（旧ranking → 6軸スコアリング） |
   | `step6` | DR向けをまとめる（旧decision → DRBFM + 判断メモ + 知見カード） |

   **追加した機能：**
   - Chart.js スパイダーチャート × 2（Step1ベンチマーク / Step3アーキテクチャー比較）
   - 状況設定バナー（`.ctx-banner`）— 各Stepに「今なぜこれをやるか」を表示
   - Step名をアクション型に変更（P0試作評価→「試作評価結果を読む」など）
   - 自然なキックオフ初期メッセージ（「この車は何で勝負するか」から入る）
   - デモスクリプトとReal AIスクリプトを新6ステップに対応

   **競合データ（Step1スパイダーチャート）：**
   - Honda E:NX2 vs Tesla Model Y / BYD Han / Hyundai IONIQ5（7軸NVスコア）
   - 弱点：後席NL(3.2)/こもり(2.9)/快適性(3.0) / 強み：風切り音(4.3)

   **アーキテクチャー比較チャート（Step3）：**
   - 前機種ベース実績 vs E:NX2目標値 vs 設計変更込み予測値（6軸）
   - DC01+DC02の複合でR02/R03達成困難を可視化

## 次にやること

- [ ] ユーザーからサンプルデータを受け取り、Step1のベンチマークスコアを実データで置換
- [ ] Step3のアーキテクチャーチャートに前機種実測データを反映
- [ ] 各Stepに「次のStepへ→」ボタンを追加（ステップナビゲーション改善）
- [ ] Step固有のクイックチップを追加（現状は汎用4チップのみ）
- [ ] Step 3 に新規追加予定ファイル `baseline_measurement.csv` / `design_change_risk.csv` の内容を反映

---

## ファイル構成（現在）

```
D:\2026\AI検討\自動車NV開発\
├── workbench_dev_log.md          ← このファイル
└── nv_ai_workbench\
    ├── nv_workbench_mockup.html  ← メインHTML（v0.2, 1084行）
    ├── README.md
    ├── AGENT.md
    ├── ROOTMAP.md
    ├── common\
    │   ├── judgment_rule.yaml
    │   ├── nv_phenomenon_catalog.csv
    │   ├── vehicle_system_catalog.csv
    │   ├── analysis_method_catalog.csv
    │   └── data_schema.md
    ├── scenarios\
    │   └── scenario_001_premium_ev_crossover\
    │       ├── scenario.yaml
    │       ├── 01_strategy\（market_trend.yaml / product_concept.yaml）
    │       ├── 02_requirement\（customer_scene.csv / nv_requirement_map.csv）
    │       ├── 03_system\（design_change.csv / vehicle_system_map.csv）
    │       ├── 04_evaluation\（measurement_result.csv / subjective_comment.md / test_plan.csv）
    │       └── 05_output\（生成物置き場）
    └── workbench\
        ├── run_workflow.md
        ├── execution_log.md
        └── prompt_templates\
```
