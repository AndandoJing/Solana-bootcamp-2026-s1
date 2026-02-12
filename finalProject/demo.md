毕业设计项目说明
本毕业设计项目同时作为 Solana Agent Hackathon 的参赛项目。

项目名称
Rule-based Escrow Agent for Bounty & Service Payments on Solana

项目代码仓库
https://github.com/AndandoJing/solana-bounty-agent

项目简介
这是一个基于 Solana 的 rule-based escrow agent，用于任务悬赏和服务支付的自动结算。
资金被锁定在链上托管账户中，并由合约根据状态与时间规则自动执行放款或退款，无需人工决策或中心化中介。

核心能力：
- Maker 创建托管并存入 Token（deposit）
- Maker 可确认交付（confirm_delivery）
- Keeper（任何人）可触发检查与执行（check_and_execute）
- 若确认交付则放款给 Keeper；若未确认且超过 deadline 则退款给 Maker
- 全流程结果由合约规则决定（deterministic）

技术介绍
- Solana + Anchor（program）
- SPL Token / Token Interface（transfer_checked / close_account）
- PDA escrow + vault ATA
- TypeScript tests（anchor test）

Demo 演示
- Repo：https://github.com/AndandoJing/solana-bounty-agent
- 视频：TODO（待补充）
