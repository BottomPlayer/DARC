---
sidebar_position: 1
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 快速开始

### 什么是 darc.js？

**darc.js** 是一个用于与 DARC 虚拟机交互的 TypeScript/JavaScript 库，适用于 node.js 和浏览器环境。

### 安装

你需要安装 **ethers.js** 5.7.2 版本或 5.x.x 版本以及 **darc.js**。

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

### 使用方法

darc.js 支持 ESM，按以下方式导入：

```ts
import * as darcjs from 'darcjs';
```

或者如果你使用 CommonJS：

```js
const darcjs = require('darcjs');
```

### 部署你的 DARC

要在兼容 EVM 的区块链上部署 DARC，你需要使用 `deployDARC(DARC_VERSION, signer)` 函数，其中 `DARC_VERSION` 是你想要部署的 DARC 版本，signer 是交易的签名者。如果你在浏览器环境中使用并想从 **MetaMask** 钱包获取签名者，可以使用 `ethers.js` 中的 `getSigner()` 函数，例如：


```ts
import { deployDARC, ethers } from 'darcjs';

// 从 MetaMask 钱包获取签名者
const signer = (new ethers.providers.Web3Provider(window.ethereum)).getSigner();

// 部署 DARC
const darc_contract_address = await darcjs.deployDARC(
  DARC_VERSION.Latest, signer
);
```

如果你在 node.js 环境中使用，需要使用 `ethers.providers` 构造一个提供者，然后使用 `ethers.js` 中的 `ethers.Wallet` 类通过私钥创建签名者，例如：

```ts
import { deployDARC, ethers } from 'darcjs';

// 通过本地开发网络节点的 JsonRpcProvider 构造提供者
const provider = new ethers.providers.JsonRpcProvider('http://127.0.0.1:8545/');

// 通过 Wallet 类和私钥构造签名者
const signer = new ethers.Wallet('0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80', provider);

// 部署 DARC
const darc_contract_address = await deployDARC(
  DARC_VERSION.Latest, signer
);

// 打印已部署的 DARC 地址
console.log("The deployed DARC address: " + darc_contract_address);
```

### 访问已部署的 DARC（只读）

要访问兼容 EVM 的区块链上已部署的 DARC，你需要通过 `DARC` 类构造一个 `DARC` 对象，构造函数接收已部署 DARC 的地址以及签名者或提供者作为参数。
你还需要指定想要访问的 DARC 版本，以及用于访问兼容 EVM 区块链的提供者。

由于你不需要向区块链写入任何内容或执行任何 DARC 程序，只需使用 `provider` 访问 DARC，不必为 DARC 构造函数使用 `signer`。

例如：

```ts
import { DARC, ethers } from 'darcjs';

// 通过本地开发网络节点的 JsonRpcProvider 构造提供者
const provider = new ethers.providers.JsonRpcProvider('http://127.0.0.1:8545/');

// 获取已部署的 DARC 地址
const darc_contract_address = '0x5FbDB2315678afecb367f032d93F642f64180aa3';

// 构造 DARC 对象
const myDARC_readOnly = new DARC({
  address: darc_contract_address,
  provider: provider,
  version: DARC_VERSION.Latest,
}); 

// 读取 DARC 的成员地址列表
const memberList = await myDARC_readOnly.getMemberList();

// 通过成员地址读取成员名称
for (let i = 0; i < memberList.length; i++) {
  const memberInfo = await myDARC_readOnly.getMemberInfo(memberList[i]);
  console.log("The member name of " + memberList[i] + " is " + memberInfo.name);
}
```

### 访问已部署的 DARC（使用签名者）

如果你想访问兼容 EVM 的区块链上已部署的 DARC，并运行程序或从 DARC 中提取一些资金，你需要使用 `signer` 通过 `DARC` 类构造 `DARC` 对象，构造函数接收已部署 DARC 的地址、签名者以及你想要访问的 DARC 版本作为参数。

由于签名者包含私钥和提供者信息，因此不必为 DARC 构造函数使用 `provider`。

以下是如何使用签名者访问已部署 DARC 的示例：

```ts
import { DARC, ethers } from 'darcjs';

// 通过本地开发网络节点的 JsonRpcProvider 构造提供者
const provider = new ethers.providers.JsonRpcProvider('http://127.0.0.1:8545/');

// 通过 Wallet 类和私钥构造签名者
const signer = new ethers.Wallet('0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80', provider);


// 获取已部署的 DARC 地址
const darc_contract_address = '0x5FbDB2315678afecb367f032d93F642f64180aa3';

// 构造 DARC 对象
const myDARC = new DARC({
  address: darc_contract_address,
  wallet: signer,    // 将签名者传递给 DARC 构造函数
  version: DARC_VERSION.Latest,
});

```

然后你可以使用 `darc` 对象访问已部署的 DARC，不仅可以读取 DARC 的数据，还可以在 DARC 上运行程序。例如：

```ts
import {transpileAndRun} from 'darcjs';
// 编写 DARC 程序的代码片段
let byLawScript = 
`
// 定义地址 1 和 2 用于铸造代币
const addr1 = '0x0000';
const addr2 = '0x0001';

// 向地址 addr1 和 addr2 铸造代币
batch_mint_tokens(
  [0,0,1,1], 
  [1000,1000,2000,2000], 
  [addr1, addr2, addr1, addr2]
  );

// 在沙箱检查操作之前启用插件 3、4
// 在沙箱检查操作之后禁用插件 5、6
batch_enable_plugins(
  [3,4,5,6],
  [true, true, false, false]
);

// 最后，向每个代币持有者分发股息
offer_dividends();
`

// 在链上部署的 DARC 上编译并运行 By-law Script 程序
const result = await myDARC.transpileAndRun(
  byLawScript,
  signer,
  dart_contract_address,
  DARC_VERSION.Latest
);
```

### 在 DARC 上执行 By-law Script 程序

`transpileAndRun` 函数用于在 DARC 上执行 By-law Script 程序。它接收 By-law Script 程序、签名者、已部署 DARC 的地址以及 DARC 版本作为参数。

以下是如何在 DARC 上执行 By-law Script 程序的示例：

```ts
import {transpileAndRun} from 'darcjs';

// 编写 DARC 程序的代码片段
let byLawScript = 
`
batch_transfer_tokens(
  ["0x0123..."],  // 接收者的地址
  [0],            // 代币索引
  [100]);         // 金额
`

// 在链上部署的 DARC 上执行 By-law Script 程序
const result = await transpileAndRun(
  byLawScript,
  signer,
  dart_contract_address,
  DARC_VERSION.Latest
);
```

`transpileAndRun` 函数包含两部分：第一部分是将 By-law Script 程序编译成操作列表，对 By-law Script 进行解析，并分别用条件节点构造函数 `or()`、`and()` 替换操作符 `|`、`&`；第二部分是在 DARC 上执行操作列表，将每个带有操作码和参数的操作推入列表，并将包含所有操作的大块对象体发送到 DARC 入口。

### 通过 SDK 执行操作列表

你也可以通过 SDK 在 DARC 上执行操作列表，无需 By-law Script 语法。`executeOperationList` 函数用于在 DARC 上执行操作列表。它接收操作列表、签名者、已部署 DARC 的地址以及 DARC 版本作为参数。你可以从 `darcjs` 库中导入所需的所有操作和插件，然后使用 `executeOperationList` 函数在 DARC 上执行操作列表。

以下是关于如何通过 SDK 执行操作列表的 TypeScript 示例：

```ts
import {
  executeOperationList, 
  batch_transfer_tokens, 
  operator_address_equals, 
  batch_add_and_enable_plugins
} from 'darcjs';


const result = executeOperationList(
  [
    // 第一个操作：批量转移代币
    batch_transfer_tokens(
      ["0x0123..."],  // 接收者的地址
      [0],            // 代币索引
      [100]),         // 金额

    batch_add_and_enable_plugins(
      [
        {
          returnType: EnumReturnType.YES_AND_SKIP_SANDBOX, // 是且跳过沙箱
          level: BigInt(103),
          votingRuleIndex: BigInt(0),
          notes: "allow operatorAddress == target1 | operatorAddress == target2",
          bIsEnabled: true,
          bIsBeforeOperation: true,
          conditionNodes: 
          or(
            operator_address_equals(target1),
            operator_address_equals(target2)
          ).generateConditionNodeList()
        }
      ]
    )
  ],
  signer,
  dart_contract_address,
  DARC_VERSION.Latest
);

```

### 开发说明：

要运行 darcjs 的测试，你需要从 GitHub 克隆 darcjs 仓库，然后使用以下命令运行测试：

```shell
git clone https://github.com/Project-DARC/DARC.git
```

然后首先为 `darc-protocol` 项目安装依赖

```shell
cd darc-protocol
npm install
```

使用以下命令启动本地开发网络节点

```shell
npx hardhat node
```

然后运行 `darc-protocol` 项目的测试

```shell
npx hardhat test
```

接着切换到 `darcjs` 项目并为 `darcjs` 项目安装依赖

```shell
cd ../darcjs
pnpm install
```

运行 `darcjs` 项目的测试

```shell
pnpm run test
```

记住，在运行 `darcjs` 项目的测试之前需要安装 `pnpm`，你可以使用以下命令安装 `pnpm`：

```shell
npm install -g pnpm
```

同时不要忘记在运行 `darcjs` 项目的测试之前初始化 `darc-protocol` 项目并启动本地开发网络节点。