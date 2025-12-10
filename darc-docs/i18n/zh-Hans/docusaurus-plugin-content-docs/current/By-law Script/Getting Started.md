---
sidebar_position: 1
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 开始

### By-law Script = JavaScript + Operator Overloading

By-law Script 是第一个用于描述基于 DARC 的加密公司的操作与规则的编程语言。它是一种领域特定语言（DSL），旨在便于阅读和编写，并可供非程序员使用。它基于 JavaScript，并通过运算符重载让读写更简单。

### Setup

编写并执行 By-law Script 程序有两种方式。第一种是使用位于 [https://darc.app](https://darc.app) 的 By-law Script IDE，这是一款基于 Web 的 IDE，允许你编写、编译并执行 By-law Script 程序。

第二种方式是使用 darcjs SDK，这是一款命令行工具，允许你在文本编辑器中编写 By-law Script 程序，然后编译并执行。

要安装 darcjs，可以使用 npm：

<Tabs>
  <TabItem value="npm" label="NPM" default>

```shell
npm install darcjs
```

  </TabItem>
  <TabItem value="yarn" label="YARN">

```shell
yarn add darcjs
```

  </TabItem>
  <TabItem value="pnpm" label="PNPM">

```shell
pnpm install darcjs
```

  </TabItem>
</Tabs>

然后你可以导入 darcjs，并使用 `transpileAndRun()` 来编译并执行 By-law Script 程序。

```javascript
import { transpileAndRun, ethers } from 'darcjs';

await transpileAndRun(

  // By-law Script 代码
  `
    batch_transfer_tokens(
      [Address_B],  // 收款地址列表
      [0],          // 代币索引
      [100]);       // 数量
  `, 

  // 签名人
  new ethers.Wallet( 
    YOUR_PRIVATE_KEY, 
    new ethers.providers.JsonRpcProvider( YOUR_JSON_RPC_PROVIDER_URL )
  ),

  // DARC 地址
  "0x123...", 

  // DARC 版本
  DARC_VERSION.Latest
);
```

### Your first By-law Script program

下面是一个简单的 By-law Script 程序，用于定义一家公司的普通股。每股普通股的投票权重为 1，分红权重为 1。该代币类别称为 `token_0`，代币索引为 0。

```javascript
batch_create_token_classes(
  ['token_0'],  // 代币名称
  [0],          // 代币索引
  [1],          // 投票权重
  [1]);         // 分红权重
```

接下来向公司创始人发行 1000 股 `token_0`，向 Address_A 发行 500 股，向 Address_B 发行 400 股，向 Address_C 发行 100 股。

```javascript
cosnt Address_A = "0x123...";
cosnt Address_B = "0x51c...";
cosnt Address_C = "0x374...";
batch_mint_tokens(
  [ Address_A, Address_B, Address_C],   // 收款地址列表
  [0,0,0],                              // 代币索引
  [500,400,100]);                       // 数量
```

随后 Address_A 在该 DARC 上执行以下 By-law Script，将 100 股 `token_0` 转给 Address_B。

```javascript
batch_transfer_tokens(
  [Address_B],  // 收款地址列表
  [0],          // 代币索引
  [100]);       // 数量
```

作为客户，Address_D 需要为服务向 DARC 支付 1000000 wei。下面的 By-law Script 由 Address_D 执行。

```javascript
pay_cash(
  1000000,  // 金额 = 1000000 wei
  0,        // 支付类型 = 0，默认原生代币
  1);       // 分红标志，1 表示该支付可分红
```

最后，Address_C 想要向所有代币持有人分红。下面的 By-law Script 由 Address_C 执行。

```javascript
offer_dividends();
```

### Package operations into a program

上面的示例代码都只执行单个操作。对于一个程序，它可以包含多个操作，所有操作将按顺序依次执行。这样做的优势在于，如果一个操作者需要执行一个由多个操作组成的程序，就必须确保该程序中的所有操作都能通过投票成功批准执行，否则整个程序将作为一个整体被拒绝。

以下是你提供示例的翻译：

**Address_E 决定投资 DARC。投资协议需要完成四项任务**：

1. Address_E 支付 50000000 wei 作为风险投资。
2. Address_E 将获得 10000 股 0 级代币（普通股）。
3. Address_E 将获得 1 股 1 级代币（董事席位）。
4. Address_E 将获得 20000 股 2 级代币（仅分红股）。"


对于上述四个操作，我们希望要么四个操作同时获得批准并成功执行，要么四个操作同时被拒绝且均不执行。对投资人 Address_E 来说，绝对不希望出现这样一种情况：单操作程序向 DARC 支付 50000000 wei，但随后 DARC 管理者否决了另外三个操作。因此，在这种场景下，这四个操作必须打包为一个程序。如果该程序需要投票，则整个程序必须经历投票流程。若被否决，Address_E 既不会获得股份或董事席位，现金也会退还给 Address_E。

下面是一个包含上述四个操作的完整 By-law Script 程序：
  
```javascript
// 向 DARC 支付 50000000 wei
pay_cash(
  50000000,  // 金额 = 50000000 wei
  0,         // 支付类型 = 0，默认原生代币
  0);        // 分红标志，0 表示支付不可分红

// 向 Address_E 铸造 10000 股 0 级、
// 1 股 1 级、
// 以及 20000 股 2 级代币
batch_mint_tokens(
  [Address_E, Address_E, Address_E],  // 收款地址列表
  [0, 1, 2],                          // 代币索引
  [10000, 1, 20000]);                 // 数量

```

该脚本将所有操作打包为一个程序，确保要么全部获批并成功执行，要么全部被拒绝且不执行。如果程序需要投票，则必须整体经过投票流程。若被拒绝，Address_E 不会获得股份或董事席位，现金也将退还给 Address_E。


### Write By-law Script, just like writing JavaScript

By-law Script 基于 JavaScript，因此大部分语法与语法规则与 JavaScript 相同，包括变量、常量和函数的使用。

下面是一个批量脚本，用于向不同地址支付不同金额。金额保存在一个 Map 中，并在循环中执行支付。

```javascript
const balance = new Map(
  [
    [address_X1, 1000000],
    [address_X2, 2000000],
    [address_X3, 3000000],
    [address_X4, 4000000]
  ]
);

for (const [address, amount] of balance) {
  batch_add_withdrawable_balances(   // 如果你不想将所有操作打包成批量操作
    [address],  // 收款地址列表
    [amount]);  // 金额
}
```

### Remember to add notes to the program

在编写 By-law Script 程序时，为程序添加注释以说明其目的非常重要，尤其在需要投票时。注释会在投票过程中展示给投票者，帮助他们理解程序目的并做出决策。

要添加注释，你可以在程序的任何位置调用 `setNote()` 函数。下面是为程序添加注释的示例。

```javascript
setNote("Investment agreement for Address_E");
```

### Your first Plugin-as-a-Law

插件是 DARC 的核心机制，也是其法律框架。DARC 内的所有规则都基于插件系统。By-law Script 支持运算符重载，使插件的组合与设计更简单便利。在 By-law Script 中，每个插件都是一个对象体。下面是最简单的示例：

```javascript
const plugin_0 = {
  returnType: NO,                  // 返回类型：NO
  level: 3,                        // 等级 3
  votingRuleIndex: 0,              // 投票规则索引，若需要投票
  notes: "disable all operations", // 插件备注
  bIsEnabled: true,                // 插件已启用，默认值
  bIsBeforeOperation: true,        // 是否在操作前执行
  conditionNodes: boolean_true()   // 条件：始终为真
},
```

在上述插件中，我们将 conditionNodes 定义为仅包含一个节点，即我们创建的 TRUE() 对象。这个插件表示在执行任何程序或操作之前都会被触发。其 returnType 为 NO，意味着只要触发该插件就会拒绝执行。因此，当该插件成功部署到 DARC 后，如果没有比等级 3 更高的插件触发以允许执行，则任何操作都会被拒绝。


By-law Script 的一个重要特性是可以通过运算符重载来编写条件节点。每个条件节点都是带参数的表达式，条件节点由 AND、OR、NOT 等逻辑运算符连接的表达式序列组成。因此，在转译器层支持运算符重载后，开发者可以轻松编写条件节点作为插件的触发条件。

下面展示了使用运算符重载与条件节点来组合插件的例子，包含四个插件，为 address_A 维护 20% 不可稀释股份制定了一套规则。

```javascript
const plugin_before_op_1 = {
  returnType: SANDBOX_NEEDED,                // 返回类型：SANDBOX_NEEDED
  level: 4,                                  // 等级 3
  votingRuleIndex: 0,                        // 投票规则索引，若需要投票
  notes: "minting tokens should be checked", // 插件备注
  bIsEnabled: true,                          // 插件已启用，默认值
  bIsBeforeOperation: true,                  // 是否在操作前执行
  conditionNodes:
    // 如果操作是铸币或创建代币类别                           
    (operation_equals(EnumOpcode.BATCH_MINT_TOKENS) |  
    operation_equals(EnumOpcode.BATCH_PAY_TO_MINT_TOKENS))

    // 且代币索引在 [0, 3] 区间，即索引为 0、1、2、3
    & batch_op_any_token_class_in_range(0, 3)
}

const plugin_after_op_1 = {
  returnType: NO,
  level: 6, 
  votingRuleIndex: 0,
  notes: "address_A must holds at least 20% of total voting and dividend weight",
  bIsEnabled: true,
  bIsBeforeOperation: false,         // 操作后插件
  conditionNodes:

    // 如果 address_A 的投票权重百分比低于 20%
    // 或 address_A 的分红权重百分比低于 20%
    address_voting_weight_percenrage_less_than(address_A, 20) 
    | address_dividend_weight_percenrage_less_than(address_A, 20)
}

const plugin_before_op_2 = {
  returnType: NO,
  level: 6, 
  votingRuleIndex: 0,
  notes: "no one can disable before-op plugin 1,2,3 or after-op plugin 1",
  bIsEnabled: true,
  bIsBeforeOperation: true,         // 操作前插件
  conditionNodes:

    operation_equals(EnumOpcode.BATCH_DISABLE_PLUGINS)
    & 
    （ disable_any_before_op_plugin_index_in_list([1,2,3])
       | disable_any_after_op_plugin_index_in_list([1]) )
    & not(operator_address_equals(address_A))
}

const plugin_before_op_3 = {
  returnType: YES_AND_SKIP_SANDBOX,
  level: 7,
  votingRuleIndex: 0,
  notes: "only address_A can disable before-op plugin 1,2,3 and after-op plugin 1",
  bIsEnabled: true,
  bIsBeforeOperation: true,         // 操作前插件
  conditionNodes: 
    operation_equals(EnumOpcode.BATCH_DISABLE_PLUGINS)
    & 
    （ disable_any_before_op_plugin_index_in_list([1,2,3])
       | disable_any_after_op_plugin_index_in_list([1]) )
    & operator_address_equals(address_A)
}
```

在上述示例中，我们定义了四个插件：

1. 第一个插件将任何 batch_mint_tokens 或 batch_pay_to_mint_tokens 操作且代币等级为 0、1、2、3 的操作标记为 SANDBOX_NEEDED。该插件确保对这些等级代币增发必须经过沙盒检查。

2. 第二个插件在任何程序执行后，如果沙盒中 address_A 的总投票或总分红权低于 20%，则将操作标记为 NO。这确保了 address_A 无论其他操作者如何增加发行，都保有 20% 的反稀释份额。

3. 第三个插件直接拒绝任何 batch_disable_plugins 操作，且被禁用的插件索引为操作前插件列表中的 1、2、3 或操作后插件列表中的 1，且操作者地址不等于 address_A。该插件确保除 address_A 之外无人能禁用这组四个插件，从而保障 address_A 的 20% 不可稀释份额。

4. 第四个插件允许任何 batch_disable_plugins 操作，且被禁用的插件索引为操作前插件列表中的 1、2、3 或操作后插件列表中的 1，且操作者地址等于 address_A。该插件确保这组四个插件可以由 address_A 禁用，使其可以通过禁用这些插件放弃反稀释特性。

定义好插件后，可以将其添加到 DARC 的插件列表中。下面是将上述四个插件添加到 DARC 插件列表的示例。确保现有的操作前插件列表仅包含 1 个插件，现有的操作后插件列表也仅包含 1 个插件。

```javascript
batch_add_and_enable_plugins(
  [plugin_before_op_1, plugin_after_op_1, plugin_before_op_2, plugin_before_op_3]);
```

### How does operator overloading work in By-law Script?

By-law Script 中的条件节点有三种类型，均继承自基类 `Node`

1. Expression：带参数的单个表达式条件节点，没有子节点，例如 `operator_address_equals(inputAddress)`。
2. Logical operators（`and`、`or`、`not`）：由逻辑运算符连接的一系列表达式条件节点。
3. Boolean constants（`boolean_true`、`boolean_false`）：布尔常量条件节点。

By-law Script 的转译器对基础 `Node` 类型支持按位与 `&` 与按位或 `|` 的运算符重载，分别重载为逻辑运算符 `and` 与 `or`。

例如，以下表达式：

```javascript
expression1() & (expression2() | expression3() ) & expression4()
```

会被转译并重载为：

```javascript
and(
  expression1(), 
  or(expression2(), expression3()),
  expression4()
)
```

由于每个小写函数实际上是对应对象构造器的包装，上述代码等价于以下代码：

```javascript
new AND(
  new Expression1(), 
  new OR(new Expression2(), new Expression3()),
  new Expression4()
)
```

当条件节点设置到插件时，运行时会自动将条件节点的树状结构序列化为节点列表（如果条件节点值的类型是 `Node`），这是插件在 DARC 接口中的实际条件节点格式。上述示例将被转译为（伪代码）：

```javascript
[
  // 节点索引 0：根节点，即 AND 节点
  {
    nodeType: "LOGICAL_OPERATOR",
    logialOperator: "AND",
    childList: [1, 2, 5]
  },

  // 节点索引 1：表达式 1
  {
    nodeType: "EXPRESSION",
    expression: "expression1"
    param: {}
    childList: []
  },

  // 节点索引 2：OR 节点
  {
    nodeType: "LOGICAL_OPERATOR",
    logialOperator: "OR",
    childList: [3, 4]
  },

  // 节点索引 3：表达式 2
  {
    nodeType: "EXPRESSION",
    expression: "expression2"
    param: {}
    childList: []
  },

  // 节点索引 4：表达式 3
  {
    nodeType: "EXPRESSION",
    expression: "expression3"
    param: {}
    childList: []
  },

  // 节点索引 5：表达式 4
  {
    nodeType: "EXPRESSION",
    expression: "expression4"
    param: {}
    childList: []
  }
]
``` 

通过这种方式，By-law Script 的转译器支持条件节点的运算符重载，运行时会生成插件在 DARC 接口中实际使用的条件节点格式。
