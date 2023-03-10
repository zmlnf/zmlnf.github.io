# 区块链基础概念

## 基本原理

- 区块：用来记录一段时间内所有的交易结果和状态结果，是对当前账本的一次共识
- 链：由区块按照发生顺序串联而成，是整个账本状态变化的日志记录
- 交易：一次对账本的操作，导致账本状态的一次改变

区块链的目标是实现一个分布的数据记录账本，这个账本只允许添加、不允许删除。账本底层的基本结构是一个线性的链表

链表由一个个“区块”串联组成（如下图所示），后继区块中记录前导区块的哈希（Hash）值。某个区块（以及块里的交易）是否合法，可通过计算哈希值的方式进行快速检验。网络中节点可以提议添加一个新的区块，但必须经过共识机制来对区块达成确认

### 区块的构成

一个区块由区块头和区块体构成

其中，区块头的内容最重要，它包括：

- 上一个区块头的 hash 值
- 时间戳
- nonce 随机数
- 难度指标
- 本区块包含所有交易信息的 Merkel Tree 的 Root hash 值
- 区块高度：可以理解为每个区块的唯一 ID，从零开始的“创世块”（即块高度为 0），一段时间生成一个块，块高度加 1

### 比特币的交易过程

1. 首先，用户通过比特币客户端发起一项交易，消息广播到比特币网络中等待确认
2. 网络中的节点会将收到的等待确认的交易请求打包在一起，添加上前一个区块头部的哈希值等信息，组成一个区块结构
3. 然后，试图找到一个 nonce 串（随机串）放到区块里，使得区块结构的哈希结果满足一定条件（比如小于某个值）。这个计算 nonce 串的过程，即俗称的“挖矿”。nonce 串的查找需要花费一定的计算力
4. 一旦节点找到了满足条件的 nonce 串，这个区块在格式上就“合法”了，成为候选区块。节点将其在网络中广播出去。其它节点收到候选区块后进行验证，发现确实合法，就承认这个区块是一个新的合法区块，并添加到自己维护的本地区块链结构上
5. 当大部分节点都接受了该区块后，意味着区块被网络接受，区块中所包括的交易也就得到确认

> 这里比较关键的步骤有两个，一个是完成对一批交易的共识（创建合法区块结构）；一个是新的区块添加到链结构上，被网络认可，确保未来无法被篡改

### 区块链的分类

#### 1.公链

公链：是指任何人都可读取的、任何人都能发送交易且交易能获得有效确认的、任何人都能参与其中共识过程的区块链。采取了工作量证明机制（POW）、权益证明机制(POS)、股份授权证明机制（DPOS）等方式，并将经济奖励和加密数字验证结合了起来，并建立一个原则就是每个人从中可获得的经济奖励与工作量成正比

特点：

- 开源
- 保护用户免受开发者的影响
- 访问门槛低
- 所有数据默认公开

#### 2.私链

私链是指其写入权限仅在一个组织手里的区块链。读取权限或者对外开放，或者被任意程度地进行了限制。相关的应用囊括数据库管理、审计、甚至一个公司，尽管在有些情况下希望它能有公共的可审计性，但在很多的情形下，公共的可读性并非是必须的

特点：

- 交易速度快：不需要每一个节点都验证交易
- 隐私性好
- 交易成本低

#### 3.联盟链

联盟链开放程度和去中心化程度是有所限制的。其参与者是被提前筛选出来或者直接指定的，数据库的读取权限可能是公开的，也可能像写入权限一样只限于系统的参与者

特点：

- 交易成本低
- 节点容易连接
- 灵活：如果需要的话，运行私有区块链的共同体或公司可以很容易地修改该区块链的规则，还原交易，修改余额等

#### 4.侧链

准确来说，侧链是一种协议，它允许资产在比特币区块链和其他区块链之间互转

简单来讲，以比特币区块链作为主链（Parent chain），其他区块链作为侧链，二者通过双向挂钩（Two-way peg），可实现比特币从主链转移到侧链进行流通

侧链可以是一个独立的区块链，有自己按需定制的账本、共识机制、交易类型、脚本和合约的支持等

侧链不能发行比特币，但可以通过支持与比特币区块链挂钩来引入和流通一定数量的比特币。当比特币在侧链流通时，主链上对应的比特币会被锁定，直到比特币从侧链回到主链

可以看到，侧链机制可将一些定制化或高频的交易放到比特币主链之外进行，实现了比特币区块链的扩展。侧链的核心原理在于能够冻结一条链上的资产，然后在另一条链上产生，可以通过多种方式来实现

特点：

- 独立性：侧链架构的好处是代码和数据独立，不增加主链的负担，避免数据过度膨胀
- 灵活性：侧链所有的区块链参数是可以定制的

## 共识机制

比特币采用了 `Proof Of Work`（POW）机制来实现共识
以太坊采用了 `Proof of Stake` （POS）机制来实现共识

### POW

POW 被称为工作量证明，通过计算来猜测一个数值（nonce），使得拼凑上交易数据后内容的 Hash 值满足规定的上限（来源于 hashcash），由于 Hash 难题在目前计算模型下需要大量的计算，这就保证在一段时间内，系统中只能出现少数合法提案

反过来，如果谁能够提出合法提案，也证明提案者确实已经付出了一定的工作量

- 优点：提供很好的抗攻击性
- 浪费计算资源

### POS

POS 被称为权益证明，类似于现实生活中的股东机制，拥有股份越多的人越容易获取记账权（同时越倾向于维护网络的正常工作）

例如：通过保证金（代币、资产、名声等具备价值属性的物品即可）来对赌一个合法的块成为新的区块，收益为抵押资本的利息和交易服务费。提供证明的保证金（例如通过转账货币记录）越多，则获得记账权的概率就越大。合法记账者可以获得收益

解决了 POW 中大量资源被浪费的问题

## 挖矿

### 概念

以比特币为例，挖矿是指网络中的维护节点，通过协助生成和确认新区块来获取一定量新增比特币的过程

目前，每 10 分钟左右生成一个不超过 1 MB 大小的区块（记录了这 10 分钟内发生的验证过的交易内容），串联到最长的链尾部，每个区块的成功提交者可以得到系统 12.5 个比特币的奖励（该奖励作为区块内的第一个交易，一定区块数后才能使用），以及用户附加到交易上的支付服务费用。即便没有任何用户交易，矿工也可以自行产生合法的区块并获得奖励

每个区块的奖励最初是 50 个比特币，每隔 21 万个区块自动减半，即 4 年时间，最终比特币总量稳定在 2100 万个

### 挖矿过程

参与者综合上一个区块的 Hash 值，上一个区块生成之后的新的验证过的交易内容，再加上自己猜测的一个随机数 X，一起打包到一个候选新区块，让新区块的 Hash 值小于比特币网络中给定的一个数，随机数越小，计算出来越难

同时，系统每隔两周（即经过 2016 个区块）会根据上一周期的挖矿时间来调整挖矿难度（通过调整限制数的大小），来调节生成区块的时间稳定在 10 分钟左右。为了避免震荡，每次调整的最大幅度为 4 倍

## 闪电网络

比特币的交易网络最为人诟病的一点便是交易性能：全网每秒 7 笔左右的交易速度，远低于传统的金融交易系统；同时，等待 6 个块的可信确认将导致约 1 个小时的最终确认时间

为了提升性能，提出了闪电网络的概念：将大量交易放到比特币区块链之外进行，只把关键环节放到链上进行确认

闪电网络主要通过引入智能合约的思想来完善链下的交易渠道。核心的概念主要有两个：RSMC（Recoverable Sequence Maturity Contract）和 HTLC（Hashed Timelock Contract）。前者解决了链下交易的确认问题，后者解决了支付通道的问题

### RSMC

Recoverable Sequence Maturity Contract，即“可撤销的顺序成熟度合同”。这个词很绕，其实主要原理很简单，类似资金池机制

首先假定交易双方之间存在一个“微支付通道”（资金池）。交易双方先预存一部分资金到“微支付通道”里，初始情况下双方的分配方案等于预存的金额。每次发生交易，需要对交易后产生资金分配结果共同进行确认，同时签字把旧版本的分配方案作废掉。任何一方需要提现时，可以将他手里双方签署过的交易结果写到区块链网络中，从而被确认。**从这个过程中可以可以看到，只有在提现时候才需要通过区块链**

### HTLC

微支付通道是通过 Hashed Timelock Contract 来实现的，中文意思是“哈希的带时钟的合约”。这个其实就是限时转账。理解起来也很简单，通过智能合约，双方约定转账方先冻结一笔钱，并提供一个哈希值，如果在一定时间内有人能提出一个字符串，使得它哈希后的值跟已知值匹配（实际上意味着转账方授权了接收方来提现），则这笔钱转给接收方

### 简述

RSMC 保障了两个人之间的直接交易可以在链下完成，HTLC 保障了任意两个人之间的转账都可以通过一条“支付”通道来完成。闪电网络整合这两种机制，就可以实现任意两个人之间的交易都在链下完成了

在整个交易中，智能合约起到了中介的重要角色，而区块链网络则确保最终的交易结果被确认

## 以太坊

以太坊区块链底层也是一个类似比特币网络的 P2P 网络平台，智能合约运行在网络中的以太坊虚拟机里。网络自身是公开可接入的，任何人都可以接入并参与网络中数据的维护，提供运行以太坊虚拟机的资源

### 核心概念

- 智能合约：一段可以复用的代码或程序，它是位于以太坊区块链上一个特定地址的一系列代码（函数）和数据（状态）
- 账户：外部账户和合约账户
- 交易：在以太坊中是指从一个账户到另一个账户的消息数据。消息数据可以是以太币或者合约执行参数
- 燃料（gas）：用来控制某次交易执行指令的上限，每执行一条合约指令会消耗固定的燃料

## 钱包

钱包(Wallet)是一个管理私钥的工具，数字货币钱包形式多样，但它通常包含一个软件客户端，允许使用者通过钱包检查、存储、交易其持有的数字货币。它是进入区块链世界的基础设施和重要入口。可以根据工作机制，分为热钱包和冷钱包

根据私钥存储方式不同，也可以分为以下几种：

- 离线钱包：离线存储私钥，也称为“冷钱包”。安全性相对最强，但无法直接发送交易，便利性差。
- 本地钱包：用本地设备存储私钥。可直接向比特币网络发送交易，易用性强，但本地设备存在被攻击风险。
- 在线钱包：用钱包服务器存储经用户口令加密过的私钥。易用性强，但钱包服务器同样可能被攻击。
- 多重签名钱包：由多方共同管理一个钱包地址，比如 2 of 3 模式下，集合三位管理者中的两位的私钥便可以发送交易。

### 内容

钱包一般包含如下内容：

- 公钥
- 私钥
- 助记词：由一些单词组成，只要你记住这些单词，按照顺序在钱包中输入，也能打开钱包
- keystore：私钥经过加密过后的一个文件，需要使用自己设置的密码才能打开

### 热钱包与冷钱包

- 热钱包是指以任何方式连入互联网的钱包
- 冷钱包与互联网完全断开。它们使用实体媒介离线存储密钥，可有效抵御黑客的在线攻击。因此，冷钱包在代币“存储”方面更具安全性。这种方式也称“冷存储”

## 密码学

### Hash 算法

Hash（哈希或散列）算法，又常被称为指纹（fingerprint）或摘要（digest）算法，可以将任意长度的二进制明文串映射为较短的（通常是固定长度的）二进制串（Hash 值），并且不同的明文很难映射为相同的 Hash 值

特点：

- 正向快速：给定原文和 Hash 算法，在有限时间和有限资源内能计算得到 Hash 值
- 逆向困难：给定（若干）Hash 值，在有限时间内无法（基本不可能）逆推出原文
- 输入敏感：原始输入信息发生任何改变，新产生的 Hash 值都应该发生很大变化
- 碰撞避免：很难找到两段内容不同的明文，使得它们的 Hash 值一致（即发生碰撞）

### 加解密算法

- 对称加密
- 非对称加密

### Merkle Tree 的结构

默克尔树（又叫哈希树）是一种典型的二叉树结构，由一个根节点、一组中间节点和一组叶节点组成

应用场景：

- 证明某个集合存在或者不存在某个元素
- 快速定位修改
- 快速比较大量数据
