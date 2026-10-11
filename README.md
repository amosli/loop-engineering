# loop-engineering · Agent 循环与 Harness 工程

> **一句话定位**：研究"LLM in a loop"这一原子结构的工程化——循环本体、上下文、验证闭环、长时运行、工具接口、安全预算。复杂度不在循环里，而在循环之外。

## 文档导航

| 文档 | 内容 |
|---|---|
| [01-技术研究规范.md](01-技术研究规范.md) | 六层规范体系：循环本体 / 上下文 / 验证 / 长时运行 / ACI / 安全预算，条款编号【L#.#】 |
| [02-最佳实践.md](02-最佳实践.md) | 30 行基线模板、Ralph 长时循环实战、各层 checklist、故障排查对照表、决策速查 |
| [03-参考资料.md](03-参考资料.md) | 30 条一手来源（含验证状态），Anthropic 工程文为主 |
| [04-国内厂商研究.md](04-国内厂商研究.md) | 国内生态：Qwen（Qwen-Agent/Code/Coder·Agent RL）、DeepSeek（DSA·官方 Harness）、MiniMax-M2（interleaved thinking）、Kimi/智谱/字节 DeerFlow、Manus 上下文工程核实、国产模型 agent 能力对照表 |

## 核心结论（2026-09 调研）

1. 循环本体只有几十行且四家 SDK 语义一致；**值得投入的是上下文工程、验证闭环、ACI 打磨与权限工程**。
2. 上下文管理的对象是 attention budget，目标是"最小的高信号 token 集"：分层加载 + compaction 白名单 + 外置进度文件 + memory tool。
3. 验证闭环是质量根源：eval 先于开发（20–50 个真实失败起步）、只评产物不评路径、pass^k 表达可靠性、0% pass@100 先怀疑 eval。
4. 长时运行的真状态放磁盘（specs / fix_plan / AGENT.md），用编译器与测试当背压，build/test 串行、搜索并行。
5. 多 agent 是例外不是默认：广度 + 可并行 + 可独立验证三者齐备才启用；token 成本 15× 于 chat。
6. 安全上 deny→ask→allow 固定顺序、无人值守必配白名单+沙箱+预算，lethal trifecta 三者不可同授。
7. **国内增量**：国产收敛到"Anthropic 协议兼容 + 模型厂薄 agent 层"（DeepSeek 已出官方 Harness）；思考帧是上下文资产（MiniMax-M2 删思考掉分）、KV-cache 命中率应入度量（Manus 10 倍价差）、长程稳定性已成模型指标（K2-Thinking 200–300 步不漂移）、基准成绩必须绑定脚手架。

## 与其他目录的关系

- **升级路径**：循环需要断点恢复 / 人工审批 / 动态 fan-out 时 → [`../graph-engineering/`](../graph-engineering/README.md)
- **应用语境**：把循环能力用于客户现场交付的角色规范 → [`../fde/`](../fde/README.md)
- **历史调研**：脚手架横向对比（Claude Code / Codex / Gemini CLI / OpenHands / Cline / Aider）→ [`../fde/02-前沿实现调研-脚手架-Loop-Graph.md`](../fde/02-前沿实现调研-脚手架-Loop-Graph.md) §1
