# Ethereum-Learn

## 简介

### 以太坊的组成部分

- P2P网络

  以太坊在以太网主网络上运行，该网络在TCP端口30303上寻址，并运行一个名为 ÐΞVp2p 的协议

- 交易（Transaction）

  以太坊交易是网络信息，其中包括发送者(sender)，接收者(receiver)，值(value)和数据的有效载荷(payload)

- 以太坊虚拟机(EVM)

  以太坊状态转换由以太坊虚拟机(EVM)处理，这是一个执行字节码（机器语言指令）的基于堆栈的虚拟机

- 数据库(Blockchain)

  以太坊的区块链作为数据库（通常是Google的LevelDB）本地存储在每个节点上，包含序列化后的交易和系统状态

- 客户端

  以太坊有几种可互操作的客户端软件实现，其中最突出的是Go-Ethereum(Geth)和Parity

### 以太坊中的重要概念

- 账户（Account）
  包含地址，余额和随机数，以及可选的存储和代码的对象。

  - 普通账户（EOA），存储和代码均为空
  - 合约账户（Contract），包含存储和代码

- 地址（Address）

  一般来说，这代表一个EOA或合约，它可以在区块链上接收或发送交易。更具体地说，它是ECDSA公钥的keccak散列的最右边的160位。

- 交易（Transaction）

  - 可以发送以太币和信息
  - 向合约发送的交易可以调用合约代码，并以信息数据为函数参数
  - 向空用户发送信息，可以自动生成以信息为代码块的合约账户

- gas

  以太坊用于执行智能合约的虚拟燃料。以太坊虚拟机使用核算机制来衡量gas的消耗量并限制计算资源的消耗。

### 以太坊的货币

以太坊的货币单位称为以太（ether），也可以表示为ETH或符号Ξ。

以太币的发行规则：

- 挖矿前（Pre-mine，Genesis）

  2014年7月/8月间，为众筹大约发行了7200万以太币。这些币有的时候被称之为“矿前”。众筹阶段之后，以太币每年的产量基本稳定，被限制不超过7200万的25%

- 挖矿产出（Mining）

  - 区块奖励（block reward）

  - 叔块奖励（uncle reward）

  - 叔块引用奖励（uncle referencing reward）

- 以太币产量未来的变化

  以太坊出块机制从工作量证明（PoW）转换为股权证明（PoS）后，以太币的发行会有什么变化尚未有定论。股权证明机制将使用一个称为Casper的协议。在Casper协议下，以太币的发行率将大大低于目前幽灵（GHOST）协议下的发行率。

### 以太坊的挖矿产出

- 区块奖励（Block rewards）

  每产生一个新区块就会有一笔固定的奖励给矿工，初始是5个以太币，现在是3个。

- 叔块奖励（Unclerewards）

  有些区块被挖得稍晚一些，因此不能作为主区块链的组成部分。比特币称这类区块为“孤块”，并且完全舍弃它们。但是，以太币称它们为“叔块”（uncles），并且在之后的区块中，可以引用它们。如果叔块在之后的区块链中作为叔块被引用，每个叔块会为挖矿者产出区块奖励的7/8。这被称之为叔块奖励。

- 叔块引用奖励（Uncle referencing rewards）

  矿工每引用一个叔块，可以得到区块奖励的1/32作为奖励（最多引用两个叔块）

- 这样的一套基于PoW的奖励机制，被称为以太坊的“幽灵协议”

### 以太坊区块收入

- 普通区块收入

  - 固定奖励（挖矿奖励），每个普通区块都有
  - 区块内包含的所有程序的gas花费的总和
  - 如果普通区块引用了叔块，每引用一个叔块可以得到固定奖励的1/32

- 叔块收入

  叔块收入只有一项，就是叔块奖励，计算公式为：

  叔块奖励 = ( 叔块高度 + 8 - 引用叔块的区块高度 ) * 普通区块奖励 / 8

### 以太坊区块链浏览器

https://etherscan.io/

### “幽灵”（GHOST）协议

- 以太坊出块时间：设计为12秒，实际14~15秒左右

- 快速确认会带来区块的高作废率，由此链的安全性也会降低

- “幽灵”协议：Greedy Heaviest Observed SubTree，"GHOST"

  - 计算工作量证明时，不仅包括当前区块的祖区块，父区块，还要包括祖先块的作废的后代区块（“叔块”），将他们进行综合考虑。

  - 目前的协议要求下探到第七层（最早的简版设计是五层），也就是说，废区块只能以叔区块的身份被其父母的第二代至第七代后辈区块引用，而不能是更远关系的后辈区块。

  - 以太坊付给以“叔区块”身份为新块确认作出贡献的废区块7/8的奖励，把它们纳入计算的“侄子区块”将获得区块奖励的1/32，不过，交易费用不会奖励给叔区块。

### 以太坊和图灵完备

- 1936年，英国数学家艾伦·图灵（Alan Turing）创建了一个计算机的数学模型，它由一个控制器、一个读写头和一根无限长的工作带组成。纸带起着存储的作用，被分成一个个的小方格（可以看成磁带）；读写头能够读取纸带上的信息，以及将运算结果写进纸带；控制器则负责根据程序对搜集到的信息进行处理。在每个时刻，机器头都要从当前纸带上读入一个方格信息，然后结合自己的内部状态查找程序表，根据程序输出信息到纸带方格上，并转换自己的内部状态，然后进行移动纸带。
- 如果个系统可以模拟任何图灵机，它就被定义为“图灵完备”（Turing Complete）的。这种系统称为通用图灵机（UTM）。
- 以太坊能够在称为以太坊虚拟机的状态机中执行存储程序，同时向内存读取和写入数据，使其成为图灵完备系统，因此成为通用图灵机。考虑到有限存储器的限制，以太坊可以计算任何可由任何图灵机计算的算法。
- 简单来说，以太坊中支持循环语句，理论上可以运行“无限循环”的程序。（停机问题）用gas解决程序无限循环问题

### 去中心化应用

- 基于以太坊可以创建智能合约（Smart Contract）来构建去中心化应用（Decentralized Application，简称为DApp）

- 以太坊的构想是成为DApps编程开发的平台

- DApp至少由以下组成：

  - 区块链上的智能合约

  - Web前端用户界面

### 以太坊应用

- 基于以太坊创建新的加密货币（Cryptocurrency，这种能力是2017年各种ICO泛滥的技术动因）
- 基于以太坊创建域名注册系统、博彩系统
- 基于以太坊开发去中心化的游戏，比如2017年底红极一时的以太猫（Cryptokities，最高单只猫售价高达80W美元）

### 代币（Token）

- 代币（token）也称作通证，本意为“令牌”，代表有所有权的资产、货币、权限等在区块链上的抽象
- 可替代性通证（fungible token）：指的是基于区块链技术发行的，互相可以替代的，可以接近无限拆分的token
- 非同质通证（non-fungible token）：指的是基于区块链技术发行的，唯一的，不可替代的，大多数情况下不可拆分的token，如加密猫（CryptoKitties）

### 名词解释

- EIP:Ethereum Improvement Proposals，以太坊改进建议
- ERC:Ethereum Request for Comments的缩写，以太坊征求意见。一些EIP被标记为ERC，表示试图定义以太坊使用的特定标准的提议
- EOA:External Owned Account，外部账户。由以太坊网络的人类用户创建的账户
- Ethash：以太坊1.0的工作量证明算法。(POW的一个具体实现)
- HD钱包：使用分层确定性（HDprotocol）密钥创建和转账协议（BIP32）的钱包。
- Keccak256：以太坊中使用的密码哈希函数。Keccak256被标准化为SHA-3
- Nonce：在密码学中，术语nonce用于指代只能使用一次的值。以太坊使用两种类型的随机数，账户随机数和POW随机数

## 一、初识以太坊——钱包、测试网络和简单交易

### 以太币单位

- 以太坊的货币单位称为以太，也称为ETH或符号Ξ

- ether被细分为更小的单位，直到可能的最小单位，称为wei；

  1 ether = 10^18 wei

- 以太的值总是在以太坊内部表示为以wei表示的无符号整数值。·以太的各种单位都有一个使用国际单位制（Sl）的科学名称，和一个口语名称。

![以太币单位](.\img\以太币单位.png)

### 以太坊钱包

以太坊钱包是我们进入以太坊系统的门户。它包含了私钥，可以代表我们创建和广播交易。

- MetaMask：一个浏览器扩展钱包，可在浏览器中运行。(做测试较方便)
- Jaxx：一款多平台、多币种的钱包，可在各种操作系统上运行，包括Android，ioS，Windows，Mac和Linux。
- MyEtherWallet（MEW）：一个基于web的钱包，可以在任何浏览器中运行。·Emerald Wallet：旨在与ETC配合使用，但与其他基于以太坊的区块链兼容。

### 私钥、公钥和地址

- 私钥（Private Key）

  以太坊私钥事实上只是一个256位的随机数，用于发送以太的交易中创建签名来证明自己对资金的所有权。

- 公钥（Public Key）

  公钥是由私钥通过椭圆曲线加密secp256k1算法单向生成的512位（64字节）数。

- 地址（Address）

  地址是由公钥的Keccak-256单向哈希，取最后20个字节（160位）派生出来的标识符。

### 安全须知

- keystore文件就是加密存储的私钥。所以当系统提示你选择密码时：
  将其设置为强密码，备份并不要共享。如果你没有密码管理器，请将其写下来并将其存放在带锁的抽屉或保险箱中。要访问账户，你必须同时有keystore文件和密码。
- 助记词可以导出私钥，所以可以认为助记词就是私钥。请使用笔和纸进行物理备份。不要把这个任务留给“以后”，你会忘记。
- 切勿以简单形式存储私钥，尤其是以电子方式存储。
- 不要将私钥资料存储在电子文档、数码照片、屏幕截图、在线驱动器、加密PDF等中。使用密码管理器或笔和纸。
- 在转移任何大额金额之前，首先要做一个小的测试交易（例如，小于1美元）。收到测试交易后，再尝试从该钱包发送。

### 安装MetaMask

- 打开Google Chrome浏览器并导航至：

  https://chrome.google.com/webstore/category/extensions

- 搜索“MetaMask”并单击狐狸的徽标。您应该看到扩展程序的详细信息页面如下：

- 验证您是否正在下载真正的MetaMask扩展程序非常重要，因为有时候人们可以通过谷歌的过滤器隐藏恶意扩展。确认您正在查看正确的扩展程序后，请点击“添加到Chrome”进行安装。

### 助记词

- 助记词是明文私钥的另一种表现形式，最早由BlP-39提出，目的是帮助用户记忆复杂的私钥（256位）。
- 技术上该提议可以在任意区块链中实现，比如使用完全相同的助记词在比特币和区块链上生成的地址可以是不同的，用户只需要记住满足一定规则的词组（就是上面说的助记词），钱包软件就可以基于该词组创建一些列的账户，并且保障不论是在什么硬件、什么时间创建出来的账户、公钥、私钥都完全相同，这样既解决了账号识记的问题，也把账户恢复的门槛降低了很多。
- 支持BIP39提议的钱包也可以归类为HD钱包（Hierarchical Deterministic Wallet），Metamask当属此类。

### 切换网络

- **Main Network（NetworkID：1）**
- 主要的、公共的，以太坊区块链。真正的ETH，真正的价值，真正的结果。
- **Ropsten Test Network（NetworkID：3）**
- 以太坊公共测试区块链和网络，使用工作量证明共识（挖矿）。该网络上的ETH没有任何价值。
- **Kovan Test Network（NetworkID：42）**
- 以太坊公共测试区块链和网络，使用“Aura”协议进行权威证明POA共识（联合签名）。该网络上的ETH没有任何价值。此测试网络仅由Parity支持。
- **Rinkeby Test Network（NetworkID：4）**
- 以太坊公共测试区块链和网络，使用“Clique”协议进行权威证明POA共识（联合签名）。该网络上的ETH没有任何价值。
- **Localhost 8545**
- 连接到与浏览器在同一台计算机上运行的节点。该节点可以是任何公共区块链（main或testnet）的一部分，也可以是私有testnet。
- **Custom RPC**
- 允许将Metamask连接到任意兼容geth的RPC接口的节点。该节点可以是任何公共或私人区块链的一部分。

### 获取测试以太

- 钱包有了，地址有了，接下来需要做的就是为我们的钱包充值。

  我们不会在主网络上这样做，因为真正的以太坊需要花钱。

- 以太坊测试网络给了我们免费获取测试以太的途径：水龙头（faucet）

- 现在，我们将尝试把一些测试以太充入我们的钱包。

- 将MetaMask切换到Ropsten测试网络。单击“Deposit”；然后单击“Ropsten Test Faucet”。

- 按绿色“request 1ether from faucet"按钮。您将在页面的下半部分看到一个交易ID。水龙头应用程序创建了一个交易-付款给您。

### 从MetaMask发送Ether

单击橙色“1ether”按钮告诉MetaMask创建支付水龙头1ether的交易。

## 二、智能合约入门

- 用Remix写一个水龙头合约

  https://remix.ethereum.org/


```solidity
pragma solidity ^0.4.17;

contract Faucet {
    function withdraw(uint amount) public {
        require(amount <= 100000000000000000000);
        msg.sender.transfer(amount);
    }
    // 回退函数 
    function () public payable{}
}
```

- 在Rinkeby Test Network上Compile后Deploy
- 在https://rinkeby.etherscan.io上查看已发布的Contract信息

## 三、以太坊客户端

### 1、简介

#### 什么是以太坊客户端

- 以太坊客户端是一个软件应用程序，它实现以太坊规范并通过p2p网络与其他以太坊客户端进行通信。如果不同的以太坊客户端符合参考规范和标准化通信协议，则可以进行相互操作。
- 以太坊是一个开源项目，由“黄皮书”正式规范定义。除了各种以太坊改进提案之外，此正式规范还定义了以太坊客户端的标准行为。
- 因为以太坊有明确的正式规范，以太网客户端有了许多独立开发的软件实现，它们之间又可以彼此交互。

#### 基于以太坊规范的网络

- 存在各种基于以太坊规范的网络，这些网络基本符合以太坊“黄皮书”中定义的形式规范，但它们之间可能相互也可能不相互操作。
- 这些基于以太坊的网络中有：以太坊，以太坊经典，Ella，Expanse，Ubiq，Musicoin等等。
- 虽然大多数在协议级别兼容，但这些网络通常具有特殊要求，以太坊客户端软件的维护人员、需要进行微小更改、以支持每个网络的功能或属性。

#### 以太坊的多种客户端

- go-ethereum（Go）

  官方推荐，开发使用最多

  地址：https://github.com/ethereum/go-ethereum

- parity（Rust）

  最轻便客户端，在历次以太坊网络攻击中表现卓越

  地址：https://github.com/ethcore/parity/releases

- cpp-ethereum（C++）

  地址：https://github.com/ethereum/cpp-ethereum

- pyethapp（python）

  地址：https://github.com/heikoheiko/pyethapp

- ethereumjs-lib（javascript）

  地址：https://github.com/ethereumis/ethereumis-lib

- EthereumJ/Harmony（Java）

  地址：https://github.com/ethereum/ethereumi

#### 以太坊全节点

- 全节点是整个主链的一个副本，存储并维护链上的所有数据，并随时验证新区块的合法性。
- 区块链的健康和扩展弹性，取决于具有许多独立操作和地理上分散的全节点。每个全节点都可以帮助其他新节点获取区块数据，并提供所有交易和合约的独立验证。
- 运行全节点将耗费巨大的成本，包括硬件资源和带宽。
- 以太坊开发不需要在实时网络（主网）上运行的全节点。我们可以使用测试网络的节点来代替，也可以用本地私链，或者使用服务商提供的基于云的以太坊客户端；这些几乎都可以执行所有操作。

#### 远程客户端和轻节点

- 远程客户端

  不存储区块链的本地副本或验证块和交易。这些客户端一般只提供钱包的功能，可以创建和广播交易。远程客户端可用于连接到现有网络，MetaMask就是一个这样的客户端。

- 轻节点

  不保存链上的区块历史数据，只保存区块链当前的状态。轻节点可以对块和交易进行验证。

#### 全节点的优缺点

优点

- 为以太坊网络的灵活性和抗审查性提供有力支持。
- 权威地验证所有交易。
- 可以直接与公共区块链上的任何合约交互。可以离线查询区块链状态（帐户，合约等）。可以直接把自己的合约部署到公共区块链中。

缺点

- 需要巨大的硬件和带宽资源，而且会不断增长。
- 第一次下载往往需要几天才能完全同步。
- 必须及时维护、升级并保持在线状态以同步区块。

#### 公共测试网络节点的优缺点

优点

- 一个testnet节点需要同步和存储更少的数据，大约10GB，具体取决于不同的网络。
- 一个testnet节点一般可以在几个小时内完全同步。
- 部署合约或进行交易只需要发送测试以太，可以从“水龙头”免费获得。
- 测试网络是公共区块链，有许多其他用户和合约运行（区别于私链）。

缺点

- 测试网络上使用测试以太，它没有价值。因此，无法测试交易对手的安全性，因为没有任何利害关系。
- 测试网络上的测试无法涵盖所有的真实主网特性。例如，交易费用虽然是发送交易所必需的，但由于gas免费，因此testnet 上往往不会考虑。而且一般来说，测试网络不会像主网那样经常拥堵。

#### 本地私链的优缺点

优点

- 磁盘上几乎没有数据，也不同步别的数据，是一个完全“干净”的环境。
- 无需获取测试以太，你可以任意分配以太，也可以随时自己挖矿获得。
- 没有其他用户，也没有其他合约，没有任何外部干扰。

缺点

- 没有其他用户意味与公链的行为不同。发送的交易并不存在空间或交易顺序的竞争。
- 除自己之外没有矿工意味着挖矿更容易预测，因此无法测试公链上发生的某些情况。
- 没有其他合约，意味着你必须部署要测试的所有内容，包括所有的依赖项和合约库。

#### 运行全节点的要求(2018年时)

最低要求

- 双核以上CPU硬盘存储可用空间至少80GB如果是SSD，需要4GB以上RAM，如果是HDD，至少8GB RAM
- 8MB/s下载带宽

推荐配置

- 四核以上的快速CPU
- 16GB以上RAM
- 500GB以上可用空间的快速SSD
- 25+MB/s下载带宽

#### Geth（Go-Ethereum）

- Geth是由以太坊基金会积极开发的Go语言实现，因此被认为是以太坊客户端的“官方”实现。

- 通常，每个基于以太坊的区块链都有自己的Geth实现。

- 以太坊的Geth github仓库链接：

  https://github.com/ethereum/go-ethereum

### 2.用Geth搭建以太坊私链

#### 2.1安装Geth

一、apt-get

```bash
sudo apt-get install software-properties-common
sudo add-apt-repository-yppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

二、源码安装（推荐）

1. 克隆github仓库

```bash
git clone https://github.com/ethereum/go-ethereum.git
```

2. 从源码构建Geth

   切换到源码目录并使用make命令

   ```bash
   cd go-ethereum
   make geth
   ```

   查看geth version ，确保在真正运行之前安装正常

   ```bash
   ./build/bin/geth version
   ```

3. 启动节点同步

