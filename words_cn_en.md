# 开发常用命名中英对照

## 常见名字

| en             | cn      | note                                                                                                                                                                                                                                                       |
|----------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| block          | 区块      |                                                                                                                                                                                                                                                            |
| block chain    | 区块链     |                                                                                                                                                                                                                                                            |
| block number   | 区块号     | 也有一种是说区块高度                                                                                                                                                                                                                                                 |
| ethereum       | 以太坊     | 简称eth                                                                                                                                                                                                                                                      |
| eth            | eth     | ethereum的token的symbol                                                                                                                                                                                                                                      |
| bitcoin        | 比特币     | 简称btc                                                                                                                                                                                                                                                      |
| btc            | btc     | bitcoin的token的symbol                                                                                                                                                                                                                                       |
| bsv            | bsv     | bitcoin sv(Satoshi Vision)，不要使用中文                                                                                                                                                                                                                          |
| bch            | bch     | bitcoin cash，比特现金                                                                                                                                                                                                                                          |
| evm            | evm     | Ethereum Virtual Machine,不建议翻译中文                                                                                                                                                                                                                           |
| jvm            | java虚拟机 |                                                                                                                                                                                                                                                            |
| wallet         | 钱包      |                                                                                                                                                                                                                                                            |
| consensus      | 共识      |                                                                                                                                                                                                                                                            |
| mnemonic       | 助记词     |                                                                                                                                                                                                                                                            |
| transaction    | 交易      |                                                                                                                                                                                                                                                            |
| tx             | 交易      | transaction的缩写                                                                                                                                                                                                                                             |
| token          | token   | 通证，直接使用英文不翻译                                                                                                                                                                                                                                               |
| eip            | eip     | Ethereum Request For Comment/以太坊意见征求稿，erc的范围更大                                                                                                                                                                                                             |
| erc            | erc     | Ethereum Improvement Proposals/以太坊改进建议                                                                                                                                                                                                                     |
| rfc            | rfc     | 请求意见稿（英语：Request for Comments，缩写：RFC）                                                                                                                                                                                                                      |
| erc20          | erc20   | [eip20](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md)                                                                                                                                                                                       |
| erc223         | erc223  | [erc223-issues](https://github.com/ethereum/eips/issues/223)                                                                                                                                                                                               |
| erc721         | erc721  | [eip721](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md)                                                                                                                                                                                     |
| erc1155        | erc1155 | [eip1155](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1155.md)                                                                                                                                                                                   |
| erc998         | erc998  | [eip998](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-998.md)                                                                                                                                                                                     |
| symbol         | symbol  | token的symbol，如eth等                                                                                                                                                                                                                                         |
| decimals       | 精度      | 就是小数后的位数，注意有s                                                                                                                                                                                                                                              |
| balance        | 余额      |                                                                                                                                                                                                                                                            |
| transfer       | 转帐      |                                                                                                                                                                                                                                                            |
| approve        |         | 允许，在token中特指，允许指定帐号地址从自己转帐的数量                                                                                                                                                                                                                              |
| value          |         | 如果使用在ethereum中，特指eth的数量                                                                                                                                                                                                                                    |
| account        |         | 在ethereum中是地址                                                                                                                                                                                                                                              |
| contract       | 智能合约    |                                                                                                                                                                                                                                                            |
| miner          | 矿工      |                                                                                                                                                                                                                                                            |
| mining machine | 矿机      |                                                                                                                                                                                                                                                            |
| Segwit         | 隔离见证    | Segregated Witness简称Segwit                                                                                                                                                                                                                                 |
| Taproot        | Taproot | In January 2018, Bitcoin developer Gregory Maxwell proposed a new BTC protocol update, called ‘Taproot’. According to his paper, the new technology will increase transaction privacy, make it more efficient, and eliminate some of SegWit’s deficiencies |
| slice          | 动态数组    | 在go与rust中的概念                                                                                                                                                                                                                                               |
| array          | 数组      | 在go与rust中都是确定大小的，在一些语言中array也是动态的                                                                                                                                                                                                                          |
| vector         | vector  | 也是一种动态数组的容器，在c++，rust中使用，不要翻译成中文。                                                                                                                                                                                                                          |
| std            | 标准"库"   | 它是standard的简称，但翻译成中文叫标准库，虽然单词中没有lib                                                                                                                                                                                                                        |
| sdk            | sdk     | software development kit，软件开发工具包，不建议翻译为中文                                                                                                                                                                                                                  |
| jdk            | jdk     | java development kit，java开发工具包，不建议翻译为中文                                                                                                                                                                                                                    |
| UB             | UB      | undefined behavior,未定义行为                                                                                                                                                                                                                                   |
| WebRTC         | WebRTC  | 全名Real-time communication for the web，是一种端到端的实时通信，主流的浏览器都支持（也有native api支持非浏览器下使用）。主要有两部分，一是建立端到端的连接，二是支持音视频及数据                                                                                                                                            |
| JNI            | JNI     | java native interface，[JNI, JNA, JNR](https://nullwy.me/2018/01/java-ffi/)                                                                                                                                                                                 |
| JNR            | JNR     | java native runtime, 性能是JNI的75%                                                                                                                                                                                                                            |
| JNA            | JNA     | java native access, JNA(Direct Mapping)性能是JNI的13%, JNA(interface Mapping)是JNI的11%                                                                                                                                                                          |
## 方法名，一般为动词

| en      | cn  | note                                      |
|---------|-----|-------------------------------------------|
| create  | 创建  | 如果new为语言的关键字，使用create命名，不能同时间使用new与create |
| new     | 构造  | 如果new为语言的关键字，使用create命名，不能同时间使用new与create |
| add     | 添加  | add主要用于增加/加入一个                            |
| remove  | 删除  |                                           |
| update  | 更新  | 有修改之意，不要使用在升级上                            |
| edit    | 修改  | update与edit都有修改之意                         |
| upgrade | 升级  |                                           |
| get     | 取得  | 单条记录                                      |
| list    | 列表  | 多条记录                                      |
| page    | 分页  |                                           |
| count   | 计数  | 也可以说统计                                    |

## 正反词

| en             | cn          | node                          |
|----------------|-------------|-------------------------------|
| add/remove     | 添加/删除       | 不要与insert/delete混用            |
| insert/delete  | 插入/删除       | 不要与add/remove混用               |
| new/delete     | 分配/释放，创建/删除 | 不要与add/remove，insert/delete混用 |
| alloc/free     | 分配/释放       | 最好不要与new/delete混用             |
| open/close     | 打开/关闭       |                               |
| begin/end      |             |                               |
| start/stop     |             |                               |
| show/hide      |             |                               |
| create/destroy |             |                               |
| source/target  |             |                               |
| first/last     |             |                               |
| min/max        |             |                               |
| get/set        |             |                               |
| up/down        |             |                               |
| old/new        |             |                               |
| next/previous  |             |                               |

## 密码加密或签名

| en                      | cn                              | note                                                                                                    |
|-------------------------|---------------------------------|---------------------------------------------------------------------------------------------------------|
| DES                     | DES                             | 数据加密标准（英语：Data Encryption Standard，缩写为 DES），改用AES                                                       |
| AES                     | AES                             | 高级加密标准（英语：Advanced Encryption Standard，缩写：AES），是用于替代DES的                                                |
| ECC                     | 椭圆曲线密码学                         | Elliptic Curve Cryptography                                                                             |
| RSA                     | RSA加密算法                         | 是三个人名的简称（Rivest-Shamir-Adleman），是一种非对称加密算法                                                              |
| DSA                     | DSA                             | 数字签名算法（Digital Signature Algorithm，DSA）                                                                 |
| ECDSA                   | ECDSA                           | 椭圆曲线数字签名算法（英语：Elliptic Curve Digital Signature Algorithm，缩写：ECDSA），是基于Weierstrass 曲线                    |
| EdDSA                   | EdDSA                           | Edwards-curve Digital Signature Algorithm (EdDSA)，是基于Edwards曲线，第二个字母d是小写。                               |
| signature               | 签名                              | 名词，表示签名后的数据                                                                                             |
| sign                    | 签名                              | 动词，表示签名的动着                                                                                              |
| verify                  | 验签                              | 验证签名，当使用在签名时                                                                                            |
| DH                      | DH                              | 迪菲-赫尔曼密钥交换（英语：Diffie–Hellman key exchange，缩写为D-H）                                                       |
| ECDH                    | ECDH                            | 椭圆曲线迪菲-赫尔曼密钥交换（英语：Elliptic Curve Diffie–Hellman key exchange，缩写为ECDH）                                   |
| secp256k1               | secp256k1                       | NIST系列曲线之一，被用来做签名算法，属于ECDSA，这条曲线也可以用于ECDH/E                                                             |
| curve25519              | curve25519                      | 25519椭圆曲线的一种，蒙哥马利曲线（Montgomery Curve），使用在ECDH/E                                                         |
| ed25519                 | ed25519                         | 使用curve25519，爱德华曲线（Edwards Curve），做签名，属于EdDSA                                                           |
| x25519                  |                                 | 把curve25519理解为一种曲线，那么x25519就是使用这条曲线来做ECDH/E的，很多时候x25519与curve25519是同一个意思                                |
| Forward Secrecy         |                                 | 前向保密（英语：Forward Secrecy，FS）                                                                             |
| Perfect Forward Secrecy |                                 | 完全前向保密（英语：Perfect Forward Secrecy，PFS）                                                                  |
| DHE                     | DHE                             | 迪菲-赫尔曼密钥交换（DHE，DH Ephemeral）,且前向安全                                                                      |
| ECDHE                   | ECDHE                           | 基于椭圆曲线迪菲-赫尔曼密钥交换（ECDHE，ECDH Ephemeral），且前向安全                                                            |
| Schnorr                 | Schnorr                         |                                                                                                         |
| MuSig                   | MuSig                           | MuSig：由Blockstream提出的Schnorr签名方案                                                                        |
| BLS                     | BLS                             | Boneh–Lynn–Shacham的简称，[see](https://www.huaweicloud.com/articles/8070926317314981b8e684133ce7ce8b.html) |
| TLS                     |                                 | 传输层安全性协议（英语：Transport Layer Security，缩写：TLS）                                                            |
| SSL                     |                                 | 安全套接层（英语：Secure Sockets Layer，缩写：SSL）                                                                   |
| CA                      |                                 | 数字证书认证机构（英语：Certificate Authority，缩写为CA）                                                                |
| digital certificate     | 数字证书                            |                                                                                                         |
| x509                    | x509                            | 是密码学里公钥证书的格式标准                                                                                          |
| TEE                     | 可信执行环境                          | Trusted Execution Environment                                                                           |
| REE                     | Rich OS Application Environment | 是与TEE相对的一个环境，或非可信执行环境，或常态运行环境                                                                           |
| TA                      | 可信应用                            | Trusted Application，它们运行在TEE下                                                                           |
| TPM                     | 可信平台模块                          | Trusted Platform Module                                                                                 |
| E2EE                    | 端到端加密                           | End-to-End encryption                                                                                   |
| X3DH                    | 密钥协商协议                          | Extended Triple Diffie-Hellman                                                                          |
| SFU                     | 选择性转发单元                         | Selective Forwarding Unit                                                                               |

## 线程或并发
| en  | cn                                 | note                 |
|-----|------------------------------------|----------------------|
| CSP | Communicating Sequential Processes | golang 就使用这种方式调度“线程” |