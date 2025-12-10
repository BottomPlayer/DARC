# 概述

### 什么是DARC?

去中心化自治监管公司(DARC)是一个用Solidity编写的虚拟机。它是一个可以部署在以太坊或EVM兼容区块链上的智能合约。它完全开源,任何人都可以在许可证下使用。

### DARC项目目标和特性

DARC项目的最终目标是设计一个全面的、自我监管的企业治理系统,使其摆脱政府和法律注册(如C类公司、有限责任公司和基金会)所施加的文本合同和外部法规的束缚。它旨在实现一个完整的、内部的、可信的和可编程的插件系统,用于DARC实例内的自我监管。因此,它具有以下特点:

1. 多级代币:DARC可以配置多个级别的代币,每个代币都有自己的投票权重和股息权重。通过各种配置和组合,不同级别的代币可以作为普通股、A/B类股票、公司债券、董事会席位、优先股,以及由复杂规则定义的各种其他公司资产和所有权结构。同样,多级代币可以是DARC销售的商品和服务,无论它们是同质化代币还是非同质化代币。

2. 插件即法律:插件是DARC协议的核心机制,管理和建立DARC内的所有规则,包括插件本身的添加、启用和禁用。插件定义了在特定条件下DARC内允许或禁止的操作。在DARC框架中,用户可以设置和添加一套完整的插件来定义DARC的日常运营,包括董事会运作、公司章程、股东协议、具有各种条件的复杂投资协议、股息规定、雇佣合同、采购合同等。通过设计全面和多样化的插件,DARC可以像现实世界的公司法律合同一样完善,规范DARC成员(如股东、董事会、员工、客户和供应商)的所有操作。

3. 操作和程序:在DARC协议的上下文中,所有操作都被抽象为一系列操作,包括操作码和参数。用户可以使用By-law Script设计、编译和运行DARC的所有日常管理和运营,包括股权操作、成员操作、现金操作、插件设计和维护、投票操作等。这些脚本在部署于区块链上的DARC虚拟机实例上部署和执行。

### DARC项目结构

整个DARC项目([https://github.com/project-darc](https://github.com/project-darc))分为四个部分:

1. DARC协议:这是DARC的核心组件,作为解释和执行所有操作和程序的虚拟机。这部分用Solidity编写和完成。用户可以选择部署预构建的DARC二进制文件,或修改和编译自己的DARC协议,一键部署到EVM兼容的区块链上。

2. By-law Script:By-law Script是一种用于编写DARC程序和插件的编程语言。By-law Script的基本语法几乎与JavaScript相同,并且它独特地支持运算符重载。因此,用户可以轻松编写复杂的布尔逻辑和全面的条件语句。By-law Script是第一种专为在法律实体内编写可执行和可部署的法律代码而设计的编程语言。

3. darcjs:darcjs是供开发者和用户与DARC协议交互的工具包和SDK。通过darcjs,用户可以轻松编译和运行By-law Script,在指定的EVM兼容区块链上部署DARC协议,或在开发者项目中嵌入与DARC相关的核心功能,如支付、投票、资产转移、法律更新、现金提取等。

4. DARC Studio:DARC Studio是一个可视化的网页前端。通过DARC Studio,用户可以在浏览器中轻松编写和运行By-law Script,无需安装darcjs。它可视化已部署的DARC实例的各种信息,如近期活动、资产、股权分配、法律文件等。

### 社区

Telegram: [https://t.me/projectdarc](https://t.me/projectdarc)

Github: [https://github.com/project-darc](https://github.com/project-darc)

Github讨论区: [https://github.com/orgs/Project-DARC/discussions](https://github.com/orgs/Project-DARC/discussions)