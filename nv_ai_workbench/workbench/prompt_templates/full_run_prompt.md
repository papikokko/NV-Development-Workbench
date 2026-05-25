# 一括実行プロンプト — Claude Code に貼り付けて使用

以下をそのままClaude Codeに貼り付けると、ワークベンチをStep 1〜6まで全実行する。

---

```
このリポジトリは、NV開発AIワークベンチのモック環境です。

目的は、現行業務プロセスをそのまま再現することではなく、市場・商品性・顧客体験・NV要求・設計部位・評価結果・判断・知見化を、AIが一気通貫でつなぐ姿を実演することです。

特に重要なのは、成果物を生成するだけではなく、各判断について必ずトレースを残すことです。

各判断に対して、以下を記録してください：
1. 判断内容
2. 使用した入力ファイル
3. 使用した具体データID
4. 使用した判断軸
5. 紐づく商品性（value_id）
6. 紐づく走行シーン（scene_id）
7. 紐づくNV要求（req_id）
8. 根拠（具体的な数値・コメント）
9. 確からしさ（High/Medium/Low）
10. 次に必要な確認

まず以下を実施してください：

1. README.md、AGENT.md、ROOTMAP.md を読んで、このワークベンチの目的と読み順を理解してください。
2. common/ 配下の共通データ構造、NV現象カタログ、車両システムカタログ、解析手法カタログ、判断ルールを理解してください。
3. scenarios/scenario_001_premium_ev_crossover/ を対象シナリオとして処理してください。
4. workbench/run_workflow.md に従い、Step 1〜6を実行して成果物を生成してください。

【生成するファイル（すべて scenarios/scenario_001_premium_ev_crossover/05_output/ に保存）】
- requirement_interpretation.md
- issue_ranking.csv
- cause_hypothesis.md
- additional_test_plan.md
- decision_memo.md
- drbfm_items.csv
- knowledge_card.md
- demo_report.md
- decision_trace.csv
- evidence_map.md
- input_usage_log.md

【注意事項】
- いきなりNV現象から判断しないでください。
- 必ず商品性と顧客体験シーンに戻して意味づけてください。
- 測定結果と官能コメントを突き合わせてください。
- 原因は断定せず、仮説と追加検証として整理してください。
- 遮音材追加だけに寄せず、源流・経路・応答・制御の観点で考えてください。
- 出力には必ず判断根拠を残してください。
- 最後に、このワークベンチが別シナリオにも使い回せるかを確認し、不足している共通データ構造があれば提案してください。
```
