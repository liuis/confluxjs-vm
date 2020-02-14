[TOC]



## 一.指令集

conflux rust： 在copy  Parity 代码的时候， 是已经兼容ethereum的几个重要的分叉，基本上是基于Istanbul 版本之后的copy， 即为ethereum Istanbul之前的 的evm 的opcode的指令集 都是支持的:

- `Byzantium 指令集`
- `Constantinople指令集`
- `Petersburg指令集` (default)
- `Istanbul（部分支持）指令集`

上述4个主要分叉都支持。未见支持MuirGlacier 分叉版本.

## 二.九类OPCode 异同：

==注意： conflux Opcode 与ethereum go/js Opcode  每个的gas fee 几乎都不相同。注意对应修改==



| conflux Opcode                 |             | ethereum go/js opecode | conflux js evm版本是否修改 |
| ------------------------------ | ----------- | ---------------------- | -------------------------- |
| **一.arithmetic ops**          |             |                        |                            |
| STOP                           | 0x00        |                        |                            |
| ADD                            | 0x01        |                        |                            |
| MUL                            | 0x02        |                        |                            |
| SUB                            | 0x03        |                        |                            |
| DIV                            | 0x04        |                        |                            |
| SDIV                           | 0x05        |                        |                            |
| MOD                            | 0x06        |                        |                            |
| SMOD                           | 0x07        |                        |                            |
| ADDMOD                         | 0x08        |                        |                            |
| MULMOD                         | 0x09        |                        |                            |
| EXP                            | 0x0a        |                        |                            |
| SIGNEXTEND                     | 0x0b        |                        |                            |
| **二.bit ops**                 |             |                        |                            |
| LT                             | 0x10        |                        |                            |
| GT                             | 0x11        |                        |                            |
| SLT                            | 0x12        |                        |                            |
| SGT                            | 0x13        |                        |                            |
| EQ                             | 0x14        |                        |                            |
| ISZERO                         | 0x15        |                        |                            |
| AND                            | 0x16        |                        |                            |
| OR                             | 0x17        |                        |                            |
| XOR                            | 0x18        |                        |                            |
| NOT                            | 0x19        |                        |                            |
| BYTE                           | 0x1a        |                        |                            |
| SHL                            | 0x1b        |                        |                            |
| SHR                            | 0x1c        |                        |                            |
| SAR                            | 0x1d        |                        |                            |
| **三.crypto**                  |             |                        |                            |
| SHA3                           | 0x20        |                        |                            |
| **四.closure state**           |             |                        |                            |
| ADDRESS                        | 0x30        |                        |                            |
| BALANCE                        | 0x31        |                        |                            |
| ORIGIN                         | 0x32        |                        |                            |
| CALLER                         | 0x33        |                        |                            |
| CALLVALUE                      | 0x34        |                        |                            |
| CALLDATALOAD                   | 0x35        |                        |                            |
| CALLDATASIZE                   | 0x36        |                        |                            |
| CALLDATACOPY                   | 0x37        |                        |                            |
| CODESIZE                       | 0x38        |                        |                            |
| CODECOPY                       | 0x39        |                        |                            |
| GASPRICE                       | 0x3a        |                        |                            |
| EXTCODESIZE                    | 0x3b        |                        |                            |
| EXTCODECOPY                    | 0x3c        |                        |                            |
| RETURNDATASIZE                 | 0x3d        |                        |                            |
| RETURNDATACOPY                 | 0x3e        |                        |                            |
| EXTCODEHASH                    | 0x3f        |                        |                            |
| **五. block operations**       |             |                        |                            |
| BLOCKHASH                      | 0x40        |                        |                            |
| COINBASE                       | 0x41        |                        |                            |
| TIMESTAMP                      | 0x42        |                        |                            |
| NUMBER                         | 0x43        |                        | 需要修改behavior           |
| DIFFICULTY                     | 0x44        |                        |                            |
| GASLIMIT                       | 0x45        |                        |                            |
|                                | ==0x46==    | ==CHAINID==            | 删掉                       |
|                                | ==0x47==    | ==SELFBALANCE==        | 删掉                       |
| **六.'storage' and execution** |             |                        |                            |
| POP                            | 0x50        |                        |                            |
| MLOAD                          | 0x51        |                        |                            |
| MSTORE                         | 0x52        |                        |                            |
| MSTORE8                        | 0x53        |                        |                            |
| SLOAD                          | 0x54        |                        |                            |
| SSTORE                         | 0x55        |                        |                            |
| JUMP                           | 0x56        |                        |                            |
| JUMPI                          | 0x57        |                        |                            |
| PC                             | 0x58        |                        |                            |
| MSIZE                          | 0x59        |                        |                            |
| GAS                            | 0x5a        |                        |                            |
| JUMPDEST                       | 0x5b        |                        |                            |
| **七.**                        |             |                        |                            |
| PUSH1 ~ PUSH32                 | 0x60~ 0x7f  |                        |                            |
| DUP1 ~ DUP16                   | 0x80~0x8f   |                        |                            |
| SWAP1 ~ SWAP16                 | 0x90 ~ 0x9f |                        |                            |
| LOG0 ~ LOG4                    | 0xa0 ~ 0xa4 |                        |                            |
| **八.closures **               |             |                        |                            |
| CREATE                         | 0xf0        |                        |                            |
| CALL                           | 0xf1        |                        |                            |
| CALLCODE                       | 0xf2        |                        |                            |
| RETURN                         | 0xf3        |                        |                            |
| DELEGATECALL                   | 0xf4        |                        |                            |
| CREATE2                        | 0xf5        |                        |                            |
| REVERT                         | 0xfd        |                        |                            |
| STATICCALL                     | 0xfa        |                        |                            |
| 九.other                       |             |                        |                            |
| ==SUICIDE==                    | ==0xff==    | ==SELFDESTRUCT==       | 修改名称                   |
|                                | ==0xfe==    | ==INVALID==            | 删掉                       |



## 三. OpCode's behavior:

以BlockNumber为例子：

#### 1.conflux  behavior:

![image-20200210220130928](/Users/liping/Library/Application Support/typora-user-images/image-20200210220130928.png)

![image-20200210220239279](/Users/liping/Library/Application Support/typora-user-images/image-20200210220239279.png)



![image-20200210220718926](/Users/liping/Library/Application Support/typora-user-images/image-20200210220718926.png)

![image-20200210220830106](/Users/liping/Library/Application Support/typora-user-images/image-20200210220830106.png)

![image-20200210220858181](/Users/liping/Library/Application Support/typora-user-images/image-20200210220858181.png)

ethereumjs  evm 是一个独立的项目，并没有相关的VM 实例化和generate block 的代码，需要回调，ethereumjs-block 项目。在其产生实例化，

#### 2.ethereumjs evm behavior:

![image-20200211015224561](/Users/liping/Library/Application Support/typora-user-images/image-20200211015224561.png)



![image-20200211015234161](/Users/liping/Library/Application Support/typora-user-images/image-20200211015234161.png)

![image-20200211015343434](/Users/liping/Library/Application Support/typora-user-images/image-20200211015343434.png)





![image-20200211015737123](/Users/liping/Library/Application Support/typora-user-images/image-20200211015737123.png)



#### 3.js 版本的使用实例：

![image-20200211020154394](/Users/liping/Library/Application Support/typora-user-images/image-20200211020154394.png)

4. #### go ethereum的behavior

![image-20200210192358507](/Users/liping/Library/Application Support/typora-user-images/image-20200210192358507.png)