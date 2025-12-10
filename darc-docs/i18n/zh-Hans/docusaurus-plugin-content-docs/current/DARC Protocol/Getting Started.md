---
sidebar_position: 1
---

# 开始

### 什么是 DARC 协议？

### 构建源码

由于使用了 Hardhat 和 OpenZeppelin，可以使用以下命令构建项目：

1. 安装依赖

   我们建议使用 `pnpm` 而不是 `npm`，但 `npm` 也可用。

   `pnpm` 是较新的包管理器，相比 npm 具有一些优势。它更快、更高效、更节省磁盘空间。

    ```shell
    cd darc-protocol
    npm install
    ```

2. 编译合约

    ```shell
    npx hardhat compile
    ```

3. 使用 Hardhat 配置启动本地 devnet 节点：

    ```shell
    npx hardhat node
    ```

4. 测试合约

    ```shell
    REPORT_GAS=true npx hardhat test --network localhost
    ```

### 部署

要部署 DARC 协议，可以使用以下命令：

```shell
npx hardhat run scripts/deploy.js --network <YOUR_NETWORK>
```

如果想将 DARC 协议部署到本地 devnet，可以使用以下命令：


```shell
npx hardhat run scripts/deploy.js --network localhost
```

在部署 DARC 协议之前，确保本地 devnet 节点已运行。

如果想将 DARC 协议部署到以太坊主网，可以使用以下命令：

```shell
npx hardhat run scripts/deploy.js --network mainnet
```
