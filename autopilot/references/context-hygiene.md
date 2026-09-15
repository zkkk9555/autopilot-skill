# context-hygiene — Lane 上下文

> 来源：瘦身前 SKILL.md §3（第 541–556 行） 逐字拆出（仅加本文件头两行）。行为以此为准，SKILL 主体只保留指针。

## 3. Context hygiene (Lane B vs Lane C vs Lane D)

- Lane B: grill → spec (+ embedded slices) in ONE unbroken window (no
  clear/compact until the spec publishes). The spec file is the seed if a
  fresh turn is ever needed.
- Lane C: the map + child tickets ARE the checkpoints. Charting may compact
  after the map is written; each closed ticket is a compact boundary.
- Lane D: exempt from the single-window rule. Diagnosis writes `NOTES.md`
  only until a fix is approved; commits happen for fixes, one per fix.
- Implement ticket-by-ticket, frontier order (blockers first), FRESH context
  per ticket = a new turn (or subagent) seeded ONLY by that ticket file. Every
  ticket must be self-contained: one-line location anchors required (see §4).
- Never resolve more than one wayfinder decision ticket per turn. Solo repos:
  research serially by default; parallel subagents write only their own ticket
  file, never `map.md` concurrently (the main loop merges).


## 并发派发纪律（V2.017）

- **回执≠证据**：子 agent 报"完成"时，主线程必须做"回执 vs 磁盘+测试"对账——文件是否真落盘、测试是否真过。只信回执 = malformed（AEC 的 CLI 票报完成实际缺 4 文件 + README，主线程按票据逐项核对才发现）。
- **合成基准唯一源头**：新验证工具必须复用已验证配方，不重写。两个配方并存且结论打架时，以已验证的为准，修新的（offline 自创配方 ERLE 仅 3.27dB，对照 test_aec 已验证配方逐行对齐后 32.20dB）。
- **验收阈值必须拿已过样本校准**：门限不许拍脑袋定——先用已 PASS 样本测出真实分布，再定门（e2e 门限两次拍脑袋都被真数据打脸，第三次用已过样本 -43.5dBFS 校准门 -50dBFS 才站住）。
- **单轮 e2e 数字不可信**：用药前先测本轮输入强度（room_capture 的耦合值就是干这个的）；跨轮波动大的指标（回声强度 -31→-18dBFS），结论必须多轮复核。
- **实时链路优先于文件验证链路**：offline 文件验证与实时链路结论打架时，以实时链路为准，文件验证链路另案再修（0.74dB FAIL vs e2e 33dB PASS）。
