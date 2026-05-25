# Step 4 プロンプトテンプレート — 課題ランキング生成

以下をClaude Codeに貼り付けて実行する。

---

```
以下のファイルを読み込んで、課題のランキングを生成してください。

【読み込むファイル】
- scenarios/scenario_001_premium_ev_crossover/04_evaluation/measurement_result.csv
- scenarios/scenario_001_premium_ev_crossover/04_evaluation/subjective_comment.md
- scenarios/scenario_001_premium_ev_crossover/02_requirement/nv_requirement_map.csv
- common/judgment_rule.yaml

【実施してほしいこと】
1. 各測定結果と官能コメントを突き合わせてください。
2. 各課題を商品性価値（value_id）への未達リスクとして解釈してください。
3. judgment_rule.yaml の各判断軸（product_value_impact / target_gap / subjective_consistency / recurrence_risk / uncertainty / schedule_impact）でスコアリングしてください。
4. 課題を優先度順にランキングし、issue_ranking.csv として出力してください。

【出力形式】
issue_ranking.csv の列：
issue_id, issue, linked_result_id, linked_comment_id, linked_value, linked_req,
product_value_impact, target_gap, subjective_consistency, recurrence_risk,
uncertainty, schedule_impact, total_score, rank, summary

【注意】
- ロードノイズが「うるさい」という理由だけで最優先にしないでください。
- 必ず商品性未達リスクの大きさを起点にランキングしてください。
- 測定値と官能コメントが不一致の場合、その旨と解釈を記録してください。
- 各スコアの根拠（どのデータを使ったか）を記録してください。
```

---

## 出力例

```csv
issue_id,issue,linked_result_id,linked_comment_id,linked_value,linked_req,product_value_impact,target_gap,subjective_consistency,recurrence_risk,uncertainty,schedule_impact,total_score,rank,summary
ISS01,後席低周波こもり（荒れ路40km/h），RSLT03,CMT02,V02/V03,R03,5,5,5,3,3,3,81,critical,目標比+4dBかつ官能で圧迫感コメントが一致。V02/V03への直接影響大。
ISS02,後席ロードノイズ（粗粒路80km/h），RSLT02,CMT01,V01,R02,5,4,4,4,3,3,76,critical,前後差3.3dBと大きく後席会話性V01を直接損なう。
```
