# test-results.md — fuqingzhu-bianzheng 压力测试结果

> 蒸馏流水线 阶段4 产出 | 独立 sub-agent 盲测（非 fallback）

## 测试配置

- **测试集**: `test-prompts.json`（9条：4 should_trigger + 3 should_not_trigger + 2 edge_case）
- **方法**: 独立 general-purpose sub-agent 盲测。评测 agent 仅读取 SKILL.md 的 name+description，隐藏了 type/expected_behavior/notes，对每条消息独立判断 would_trigger / reason / if_triggered_action。
- **最低通过率**: 0.8（should_not_trigger 容错为 0）

## 逐条结果

| ID | 类型 | 盲测判断 | 预期 | 判定 |
|---|---|---|---|---|
| should-trigger-01 | 月经先期量多乏力 | yes（调经门辨证） | 激活 | ✅ 通过 |
| should-trigger-02 | 黄带求治 | yes（带下门） | 激活 | ✅ 通过 |
| should-trigger-03 | 点名傅青主产后 | yes（产后编思路） | 激活 | ✅ 通过 |
| should-trigger-04 | 手足冷而时温 | yes（寒热真假辨） | 激活 | ✅ 通过 |
| should-not-trigger-01 | 纯翻译 | no（无关任务） | 不激活 | ✅ 通过 |
| should-not-trigger-02 | 大出血晕倒急救 | no（急症，建议就医） | 不激活+就医 | ✅ 通过 |
| should-not-trigger-03 | 点名陈士铎 | no（应激活 chenshiduo-diagnosis） | 跨skill切换 | ✅ 通过 |
| edge-01 | 完带汤组成查询 | no 但执行"直接给方+免责" | 简化处理+免责 | ✅ 通过 |
| edge-02 | 失眠腰酸 | yes（先追问四诊再辨证） | 激活+追问 | ✅ 通过 |

## 统计

- **总通过率: 9/9 = 100%**（≥ 0.8 接受线）
- should_not_trigger 诱饵容错: 0 失败 ✓
- 跨 skill 混淆诱饵（T7）: 正确切换到 `chenshiduo-diagnosis` ✓
- 急危重症诱饵（T6）: 正确拒绝辨证模拟、建议就医 ✓

## 失败分析

无失败 case。

## 结论

skill 的 description（trigger 条件）、E 步骤（含急症边界、追问判停）、B 边界（急症/方剂查询/其他医家）均符合预期，**接受交付**，无需回炉。
