# 毕业设计项目说明
本毕业设计项目同时作为 Solana Agent Hackathon 的参赛项目（后续将提交到 4 月全球黑客松，链接待补）。

---

## 项目名称
Rule-based Escrow Agent for Bounty & Service Payments on Solana  
（基于规则的托管结算 Agent：用于悬赏与服务支付）

---

## 项目代码仓库
https://github.com/AndandoJing/solana-bounty-agent

---

## 项目简介
这是一个基于 Solana 的 **rule-based escrow agent**，用于任务悬赏和服务支付的自动结算。

资金先被锁定在链上托管账户（Escrow PDA + Vault ATA）中，随后由合约根据 **链上状态 + 截止时间（deadline）** 自动决定：
- **Release（放款）**：maker 已确认交付（buyer_confirmed = true）→ keeper 可触发执行 → 资金发给 keeper（示例路径）
- **Refund（退款）**：未确认交付且 deadline 已过 → keeper 可触发执行 → 资金退回 maker

整个执行过程不依赖中心化仲裁或人工判断：**任何人都可以触发执行，但执行结果由合约规则决定**，符合可审计、确定性、可复现的 Agent 结算模式。

---

## 技术介绍（核心点）
- **Anchor Framework**：实现指令与账户约束（make / confirm_delivery / check_and_execute / take / refund）
- **PDA + Seeds**：Escrow 使用 `["escrow", maker, seed]` 派生，避免地址冲突，可支持同一 maker 多笔托管
- **SPL Token / TokenInterface**：使用 `transfer_checked` 托管转账；执行后使用 `close_account` 回收 vault
- **ATA 处理**：使用 Associated Token Program 创建/校验 maker/taker/keeper 的 ATA
- **Agent 执行模式**：keeper（或任何人）只负责 trigger，不参与决策；合约内部 enforce 规则（confirmed 或 deadline）
- **测试覆盖**：包含 release path 与 refund path 两条主路径的集成测试（Anchor ts-mocha）

---

## Demo 演示
- 本地测试（已通过）：
  - `anchor test --skip-local-validator`
  - 覆盖：release path + refund path
- 视频链接：TODO（下一步将补录屏并更新）

---

## 你可以如何运行（本地）
```bash
git clone https://github.com/AndandoJing/solana-bounty-agent
cd solana-bounty-agent
anchor build
anchor test --skip-local-validator
