# ROOTMAP.md

## AIの実行時読み順

AIはこのワークベンチを実行する際、以下の順序でファイルを読み込むこと。

```
1.  README.md                                        ← 目的・基本導線の確認
2.  AGENT.md                                         ← 行動規約の確認
3.  ROOTMAP.md（本ファイル）                          ← 構造の確認
4.  common/data_schema.md                            ← データ接続ルール
5.  common/judgment_rule.yaml                        ← 判断軸・重み
6.  common/nv_phenomenon_catalog.csv                 ← NV現象辞書
7.  common/vehicle_system_catalog.csv                ← 車両システム辞書
8.  common/analysis_method_catalog.csv               ← 解析手法辞書
9.  scenarios/{target}/scenario.yaml                 ← 対象シナリオの概要
10. scenarios/{target}/01_strategy/market_trend.yaml ← 市場・技術トレンド
11. scenarios/{target}/01_strategy/product_concept.yaml ← 商品性定義
12. scenarios/{target}/02_requirement/customer_scene.csv ← 顧客体験シーン
13. scenarios/{target}/02_requirement/nv_requirement_map.csv ← NV要求マップ
14. scenarios/{target}/03_system/vehicle_system_map.csv ← 車両システム関係
15. scenarios/{target}/03_system/design_change.csv   ← 設計変更点
16. scenarios/{target}/04_evaluation/test_plan.csv   ← 評価計画
17. scenarios/{target}/04_evaluation/measurement_result.csv ← 評価結果
18. scenarios/{target}/04_evaluation/subjective_comment.md ← 官能コメント
19. workbench/run_workflow.md                        ← 実行手順
```

## 各フォルダの意味

| パス | 意味 | 備考 |
|------|------|------|
| `common/` | シナリオ共通の辞書・判断ルール | 変更時は全シナリオに影響 |
| `common/output_template/` | 出力ファイルのテンプレート | 成果物の書式を統一 |
| `scenarios/` | 個別シナリオのデータ置き場 | 差し替えてもワークベンチ本体は変わらない |
| `scenarios/{name}/01_strategy/` | 市場・商品戦略の入力データ | |
| `scenarios/{name}/02_requirement/` | 顧客シーン・NV要求の入力データ | |
| `scenarios/{name}/03_system/` | 車両システム・設計変更の入力データ | |
| `scenarios/{name}/04_evaluation/` | 評価計画・評価結果・官能コメント | |
| `scenarios/{name}/05_output/` | AI生成の成果物（毎回上書き） | |
| `workbench/` | 実行手順・プロンプト・ログ | シナリオ共通 |
| `workbench/prompt_templates/` | 各Stepのプロンプトひな形 | |

## 重要

出力時は、通常成果物に加えて以下を必ず作成すること：

- `05_output/decision_trace.csv`
- `05_output/evidence_map.md`
- `05_output/input_usage_log.md`
- `workbench/execution_log.md`（実行のたびに追記）
