# AI実行ワークフロー

## 実行前準備

1. ROOTMAP.md に従い、ファイルを読み込み順序で確認する
2. 対象シナリオを特定する（例：`scenarios/scenario_001_premium_ev_crossover/`）
3. `workbench/execution_log.md` に実行日・シナリオを記録する

---

## Step 1 — 市場・商品性の解釈

**主な入力**
- `01_strategy/market_trend.yaml`
- `01_strategy/product_concept.yaml`

**実施内容**
- 市場トレンドと商品性価値の関係を整理する
- NV開発において何が最も重要な商品性課題かを言語化する
- 「遮音材を足せばよい」という発想に陥っていないかを確認する

**出力**
- `05_output/requirement_interpretation.md`
- `05_output/input_usage_log.md`（Step 1分）

**トレース記録**
- 使用したtend_id、value_idを記録する
- NV開発の重点として何を選んだか、なぜかを記録する

---

## Step 2 — 商品性からNV要求への翻訳

**主な入力**
- `02_requirement/customer_scene.csv`
- `02_requirement/nv_requirement_map.csv`
- `common/nv_phenomenon_catalog.csv`

**実施内容**
- 各商品性価値を顧客体験シーンに分解する
- シーンからNV要求・NV現象・関連部位へ接続する
- 要求の優先順位を商品性起点で判断する

**出力**
- `05_output/requirement_interpretation.md`（補足）
- `05_output/input_usage_log.md`（Step 2分）

**トレース記録**
- value_id → scene_id → req_id の接続を記録する

---

## Step 3 — 評価計画の確認

**主な入力**
- `02_requirement/nv_requirement_map.csv`
- `04_evaluation/test_plan.csv`
- `03_system/design_change.csv`

**実施内容**
- 評価計画がNV要求と設計変更を適切にカバーしているか確認する
- 設計変更に対して比較評価（仕様違い比較）が計画されているか確認する
- カバーできていない要求・変更点を指摘する

**出力**
- `05_output/input_usage_log.md`（Step 3分）

**トレース記録**
- req_id → test_id の対応関係を確認する

---

## Step 4 — 評価結果レビューと課題ランキング

**主な入力**
- `04_evaluation/measurement_result.csv`
- `04_evaluation/subjective_comment.md`
- `02_requirement/nv_requirement_map.csv`
- `common/judgment_rule.yaml`

**実施内容**
- 測定結果と官能コメントを突き合わせる
- 各課題を商品性価値未達リスクとして解釈する
- 判断ルール（judgment_rule.yaml）の各軸でスコアリングする
- 課題を優先度順にランキングする

**出力**
- `05_output/issue_ranking.csv`
- `05_output/decision_trace.csv`（Step 4分）
- `05_output/input_usage_log.md`（Step 4分）

**トレース記録**
- result_id・コメントIDと判断軸を対応させて記録する

---

## Step 5 — 原因仮説と追加検証計画

**主な入力**
- `05_output/issue_ranking.csv`
- `03_system/design_change.csv`
- `03_system/vehicle_system_map.csv`
- `common/vehicle_system_catalog.csv`
- `common/analysis_method_catalog.csv`

**実施内容**
- 上位課題について原因仮説を複数立てる（断定しない）
- 各仮説に対して必要な追加検証を定義する
- 設計変更との関係を整理する
- 遮音材追加以外の対策観点（源流・経路・応答・制御）を検討する

**出力**
- `05_output/cause_hypothesis.md`
- `05_output/additional_test_plan.md`
- `05_output/evidence_map.md`
- `05_output/decision_trace.csv`（Step 5分）

**トレース記録**
- issue_id → system_id → method_id → hypothesis の接続を記録する

---

## Step 6 — 判断出力生成

**主な入力**
- `05_output/issue_ranking.csv`
- `05_output/cause_hypothesis.md`
- `05_output/additional_test_plan.md`

**実施内容**
- 開発判断メモを作成する（次のDRに向けた方針）
- DRBFMアイテムを生成する（設計変更起点）
- 知見カードを作成する（次シナリオへの引き継ぎ）
- デモ用レポートを作成する

**出力**
- `05_output/decision_memo.md`
- `05_output/drbfm_items.csv`
- `05_output/knowledge_card.md`
- `05_output/demo_report.md`
- `05_output/decision_trace.csv`（Step 6分・完成版）
- `05_output/evidence_map.md`（完成版）
- `05_output/input_usage_log.md`（全Step完成版）
- `workbench/execution_log.md`（実行記録追記）

---

## 実行チェックリスト

| 確認項目 | OK基準 |
|---------|--------|
| 商品性起点になっているか | 市場・商品性からNV要求に落ちている |
| 特性起点に戻っていないか | ロードノイズ/こもり音は商品性から分解された結果として出ている |
| 判断線が見えるか | 商品性→シーン→NV現象→部位→評価→判断がつながる |
| 判断根拠が追えるか | decision_trace.csv に使用入力・データID・判断軸が残っている |
| AIが作文だけしていないか | issue_ranking, DRBFM, 追加検証に根拠がある |
| 遮音材追加だけに寄っていないか | 源流・経路・応答・制御の観点が含まれている |
