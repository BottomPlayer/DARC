---
sidebar_position: 2
---

# 操作码

### 什么是操作码?

Decentralized Autonomous Regulated Corporation（DARC）是一种协议，可用于创建和管理具备内置治理与监管机制的自治实体。DARC 协议的关键组件之一是 opcode 指令集，它定义了在系统内管理代币、资金与规则的基础操作。

opcode 指令集为与 DARC 协议交互并执行各类动作提供了标准化方式。这些指令定义了 DARC 参与者可以执行的操作，支持创建、转移、销毁代币，以及管理成员、角色和插件。

通过 opcode 指令集，用户可以铸造新代币、创建具有特定属性（如投票权重和分红权重）的代币类别、在地址间转移代币、并销毁代币以将其从流通中移除。它还支持添加与暂停成员、修改成员角色与名称，以及启用或禁用插件以获得额外功能。

此外，opcode 指令集包含处理资金交易的操作，例如将现金提取至指定地址、支付现金、发放股息，以及为所有操作设置审批。它还提供添加存储 IPFS 哈希、对提案进行投票、执行程序，以及在 DARC 生态中执行其他关键功能的机制。

通过以 opcode 定义这些操作，DARC 协议确保在自治实体内管理代币、资金与规则的过程保持一致、透明且安全。这些指令构成去中心化公司治理与监管的基础，为透明、可审计的决策流程提供框架。

### 说明

在 DARC 协议中，操作名称使用全大写；在 By-law Script 与 darcjs SDK 中，操作名称使用全小写。例如，DARC 协议中的操作 `BATCH_MINT_TOKENS` 在 By-law Script 与 darcjs SDK 中均写作 `batch_mint_tokens`。

| Opcode ID | Opcode Name                                  | Opcode Parameter                                        | Opcode Notes                                                |
|-----------|----------------------------------------------|----------------------------------------------------------|------------------------------------------------------------|
| 0         | UNDEFINED                                    |                                                          | 无效操作                                                    |
| 1         | BATCH_MINT_TOKENS                            | ADDRESS_2DARRAY[0] toAddressArray, UINT256_2DARRAY[0] tokenClassArray, UINT256_2DARRAY[1] amountArray | 批量铸造代币操作                                            |
| 2         | BATCH_CREATE_TOKEN_CLASSES                   | STRING_ARRAY[] nameArray, UINT256_2DARRAY[0] tokenIndexArray, UINT256_2DARRAY[1] votingWeightArray, UINT256_2DARRAY[2] dividendWeightArray | 批量创建代币类别操作                                        |
| 3         | BATCH_TRANSFER_TOKENS                        | ADDRESS_2DARRAY[0] toAddressArray, UINT256_2DARRAY[0] tokenClassArray, UINT256_2DARRAY[1] amountArray | 批量转移代币操作                                            |
| 4         | BATCH_TRANSFER_TOKENS_FROM_TO                | ADDRESS_2DARRAY[0] fromAddressArray, ADDRESS_2DARRAY[1] toAddressArray, UINT256_2DARRAY[0] tokenClassArray, UINT256_2DARRAY[1] amountArray | 批量从地址 A 转移代币至地址 B 操作                          |
| 5         | BATCH_BURN_TOKENS                            | UINT256_2DARRAY[0] tokenClassArray, UINT256_2DARRAY[1] amountArray | 批量销毁代币操作                                            |
| 6         | BATCH_BURN_TOKENS_FROM                       | ADDRESS_2DARRAY[0] fromAddressArray, UINT256_2DARRAY[0] tokenClassArray, UINT256_2DARRAY[1] amountArray | 批量从指定地址销毁代币操作                                  |
| 7         | BATCH_ADD_MEMBERSHIPS                         | ADDRESS_2DARRAY[0] memberAddressArray, UINT256_2DARRAY[0] memberRoleArray, STRING_ARRAY memberNameArray | 批量添加成员操作                                            |
| 8         | BATCH_SUSPEND_MEMBERSHIPS                     | ADDRESS_2DARRAY[0] memberAddressArray                     | 批量暂停成员操作                                            |
| 9         | BATCH_RESUME_MEMBERSHIPS                      | ADDRESS_2DARRAY[0] memberAddressArray                     | 批量恢复成员操作                                            |
| 10        | BATCH_CHANGE_MEMBER_ROLES                    | ADDRESS_2DARRAY[0] memberAddressArray, UINT256_2DARRAY[0] memberRoleArray | 批量变更成员角色操作                                        |
| 11        | BATCH_CHANGE_MEMBER_NAMES                    | ADDRESS_2DARRAY[0] memberAddressArray, STRING_ARRAY memberNameArray | 批量变更成员名称操作                                        |
| 12        | BATCH_ADD_PLUGINS                            | Plugin[] pluginList                                     | 批量添加紧急代理操作                                        |
| 13        | BATCH_ENABLE_PLUGINS                         | UINT256_ARRAY[0] pluginIndexArray, BOOL_ARRAY isBeforeOperationArray | 批量启用插件操作                                            |
| 14        | BATCH_DISABLE_PLUGINS                        | UINT256_ARRAY[0] pluginIndexArray, BOOL_ARRAY isBeforeOperationArray | 批量禁用插件操作                                            |
| 15        | BATCH_ADD_AND_ENABLE_PLUGINS                | Plugin[] pluginList                                     | 批量添加并启用插件操作                                      |
| 16        | BATCH_SET_PARAMETERS                         | MachineParameter[] parameterNameArray, UINT256_2DARRAY[0] parameterValueArray | 批量设置参数操作                                            |
| 17        | BATCH_ADD_WITHDRAWABLE_BALANCES              | ADDRESS_2DARRAY[0] addressArray, UINT256_2DARRAY[0] amountArray | 批量增加可提取余额操作                                      |
| 18        | BATCH_REDUCE_WITHDRAWABLE_BALANCES           | ADDRESS_2DARRAY[0] addressArray, UINT256_2DARRAY[0] amountArray | 批量减少可提取余额操作                                      |
| 19        | BATCH_ADD_VOTING_RULES                       | VotingRule[] votingRuleList                              | 批量添加投票规则操作                                        |
| 20        | BATCH_PAY_TO_MINT_TOKENS                     | ADDRESS_2DARRAY[0] addressArray, UINT256_2DARRAY[0] tokenClassArray, UINT256_2DARRAY[1] amountArray, UINT256_2DARRAY[2] priceArray, UINT256_2DARRAY[3] dividendableFlag | 批量付费铸造代币操作                                        |
| 21        | BATCH_PAY_TO_TRANSFER_TOKENS                 | ADDRESS_2DARRAY[0] toAddressArray, UINT256_2DARRAY[0] tokenClassArray, UINT256_2DARRAY[1] amountArray, UINT256_2DARRAY[2] priceArray, UINT256_2DARRAY[3] dividendableFlag | 付费转移代币操作（产品币）                                   |
| 22        | ADD_EMERGENCY                               | ADDRESS_2DARRAY[0] addressArray                           | 添加一组地址为紧急代理的操作                                |
| 23        | RESERVED_ID_23                               |                                                          | 预留 ID 23，勿使用                                           |
| 24        | CALL_EMERGENCY                               | UINT256_2DARRAY[0][0] emergencyAgentIndex                           | 调用紧急代理以处理紧急情况的操作                            |
| 25        | CALL_CONTRACT_ABI                            | ADDRESS_2D[0][0] contractAddress, bytes abi, UINT256_2DARRAY[0][0] value | 以给定 ABI 调用合约的操作                                   |
| 26        | PAY_CASH                                     | uint256 amount, uint256 paymentType, uint256 dividendable | 支付现金操作                                                |
| 27        | OFFER_DIVIDENDS                              |                                                          | 计算并向代币持有人发放股息的操作                            |
| 28        | RESERVED_ID_28                               |                                                          | 预留 ID 28，勿使用                                           |
| 29        | SET_APPROVAL_FOR_ALL_OPERATIONS              | ADDRESS_2DARRAY[0][0] targetAddress                                          | 按地址为所有转移操作设置批准的操作                           |
| 30        | BATCH_BURN_TOKENS_AND_REFUND                 | UINT256_2D[0] tokenClassArray, UINT256_2D[1] amountArray, UINT256_2D[2] priceArray | 批量销毁代币并退款操作                                      |
| 31        | ADD_STORAGE_STRING                        | STRING_ARRAY[0][0] IFPSHash                              | 将存储 IPFS 哈希永久加入存储列表的操作                      |
| 32        | VOTE                                         | bool[] voteArray                                       | 对待决程序进行投票的操作                                    |
| 33        | EXECUTE_PENDING_PROGRAM                               |                                                          | 执行已获投票批准的程序操作                                  |
| 34        | END_EMERGENCY                                |                                                          | 结束紧急模式操作                                            |
| 35        | UPGRADE_TO_ADDRESS                           | ADDRESS_2DARRAY[0][0] targetAddress  | 将合约升级到新合约地址的操作                                |
| 36        | CONFIRM_UPGRAED_FROM_ADDRESS                 | ADDRESS_2DARRAY[0][0] fromAddress   | 接受当前 DARC 从旧合约地址升级而来的操作                    |
