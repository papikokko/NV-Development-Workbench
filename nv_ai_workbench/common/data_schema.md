# Data Schema

## 基本階層（上位から下位へ）

```
market_trend
  ↓
product_value
  ↓
customer_scene
  ↓
nv_requirement
  ↓
nv_phenomenon
  ↓
vehicle_system
  ↓
test_plan
  ↓
measurement_result
  ↓
issue
  ↓
cause_hypothesis
  ↓
action
  ↓
decision_output
```

## IDテーブル

| ID名 | 意味 | 接続先 |
|-----|------|-------|
| `trend_id` | 市場・技術トレンド | `product_value` |
| `value_id` | 商品性価値 | `customer_scene`, `nv_requirement` |
| `scene_id` | 顧客体験シーン | `nv_requirement`, `test_plan` |
| `req_id` | NV要求 | `nv_phenomenon`, `vehicle_system` |
| `phenomenon_id` | NV現象 | `vehicle_system`, `test_plan` |
| `system_id` | 車両システム・設計部位 | `cause_hypothesis` |
| `test_id` | 評価条件 | `measurement_result` |
| `result_id` | 評価結果 | `issue`, `decision_trace` |
| `issue_id` | 課題 | `cause_hypothesis`, `action` |
| `decision_id` | 判断 | `decision_trace`, `evidence_map` |

## 接続ルール

1. NV要求は必ず `product_value` と `customer_scene` に紐づける。
2. 評価結果は必ず `test_id` に紐づける。
3. 課題は必ず `product_value` に戻して評価する（商品性未達リスクとして解釈）。
4. 原因仮説は `vehicle_system` と `analysis_method` に紐づける。
5. DRBFM項目は `design_change` を起点に作成する。
6. 判断は必ず `decision_trace.csv` に記録する。

## 禁止パターン

- NV現象だけを起点にした課題設定（必ず商品性まで遡ること）
- result_idなしの評価結果参照
- value_idなしの課題設定
- 原因断定（仮説として必ず確からしさを付記する）
