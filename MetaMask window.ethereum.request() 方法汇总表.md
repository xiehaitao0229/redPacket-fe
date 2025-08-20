# MetaMask window.ethereum.request() 方法汇总表

## 账户相关方法 (Account Methods)

| 方法名                      | 作用                           | 参数                     | 返回值                    | 备注                                         |
| --------------------------- | ------------------------------ | ------------------------ | ------------------------- | -------------------------------------------- |
| `eth_requestAccounts`       | 请求用户连接钱包，获取账户权限 | 无                       | `string[]` - 账户地址数组 | 会弹出MetaMask授权窗口，用户可选择连接的账户 |
| `eth_accounts`              | 获取当前已连接的账户列表       | 无                       | `string[]` - 账户地址数组 | 不会弹窗，只返回已授权的账户                 |
| `wallet_requestPermissions` | 请求特定权限                   | `[{ eth_accounts: {} }]` | `Web3WalletPermission[]`  | 更细粒度的权限控制                           |
| `wallet_getPermissions`     | 获取当前权限列表               | 无                       | `Web3WalletPermission[]`  | 查看当前拥有的权限                           |
| `wallet_revokePermissions`  | 撤销特定权限                   | `[{ eth_accounts: {} }]` | `null`                    | 撤销指定权限                                 |

## 交易相关方法 (Transaction Methods)

| 方法名                      | 作用                 | 参数                                         | 返回值                    | 备注                   |
| --------------------------- | -------------------- | -------------------------------------------- | ------------------------- | ---------------------- |
| `eth_sendTransaction`       | 发送交易             | `[{ from, to, value, gas, gasPrice, data }]` | `string` - 交易哈希       | 会弹出MetaMask确认窗口 |
| `eth_sendRawTransaction`    | 发送已签名的原始交易 | `[signedTransactionData]`                    | `string` - 交易哈希       | 发送预签名交易         |
| `eth_estimateGas`           | 估算交易所需Gas      | `[transactionObject]`                        | `string` - Gas估算值      | 预估交易成本           |
| `eth_gasPrice`              | 获取当前Gas价格      | 无                                           | `string` - Gas价格（wei） | 获取网络Gas价格        |
| `eth_getTransactionByHash`  | 通过哈希获取交易信息 | `[transactionHash]`                          | `object` - 交易对象       | 查询交易详情           |
| `eth_getTransactionReceipt` | 获取交易收据         | `[transactionHash]`                          | `object` - 交易收据       | 获取交易执行结果       |

## 签名相关方法 (Signing Methods)

| 方法名                 | 作用                       | 参数                   | 返回值              | 备注                     |
| ---------------------- | -------------------------- | ---------------------- | ------------------- | ------------------------ |
| `personal_sign`        | 个人消息签名               | `[message, address]`   | `string` - 签名结果 | 最常用的签名方法         |
| `eth_sign`             | 原始消息签名               | `[address, message]`   | `string` - 签名结果 | 不推荐使用，存在安全风险 |
| `eth_signTypedData_v4` | 结构化数据签名（v4）       | `[address, typedData]` | `string` - 签名结果 | EIP-712标准，最安全      |
| `eth_signTypedData_v3` | 结构化数据签名（v3）       | `[address, typedData]` | `string` - 签名结果 | 较旧版本                 |
| `eth_signTypedData`    | 结构化数据签名（原始版本） | `[address, typedData]` | `string` - 签名结果 | 已弃用                   |

## 网络/链相关方法 (Network/Chain Methods)

| 方法名                       | 作用         | 参数                                                         | 返回值                      | 备注               |
| ---------------------------- | ------------ | ------------------------------------------------------------ | --------------------------- | ------------------ |
| `eth_chainId`                | 获取当前链ID | 无                                                           | `string` - 链ID（十六进制） | 如 "0x1" (Mainnet) |
| `wallet_switchEthereumChain` | 切换网络     | `[{ chainId }]`                                              | `null`                      | 切换到指定网络     |
| `wallet_addEthereumChain`    | 添加新网络   | `[{ chainId, chainName, nativeCurrency, rpcUrls, blockExplorerUrls }]` | `null`                      | 添加自定义网络     |
| `net_version`                | 获取网络版本 | 无                                                           | `string` - 网络版本         | 获取网络ID         |

## 资产相关方法 (Asset Methods)

| 方法名              | 作用           | 参数                                                         | 返回值                 | 备注          |
| ------------------- | -------------- | ------------------------------------------------------------ | ---------------------- | ------------- |
| `wallet_watchAsset` | 添加代币到钱包 | `[{ type: "ERC20", options: { address, symbol, decimals, image } }]` | `boolean`              | 添加ERC20代币 |
| `eth_getBalance`    | 获取账户余额   | `[address, blockParameter]`                                  | `string` - 余额（wei） | 获取ETH余额   |

## 区块链查询方法 (Blockchain Query Methods)

| 方法名                 | 作用                     | 参数                                        | 返回值              | 备注                |
| ---------------------- | ------------------------ | ------------------------------------------- | ------------------- | ------------------- |
| `eth_blockNumber`      | 获取最新区块号           | 无                                          | `string` - 区块号   | 获取当前区块高度    |
| `eth_getBlockByNumber` | 通过区块号获取区块       | `[blockNumber, fullTxObjects]`              | `object` - 区块对象 | 查询指定区块        |
| `eth_getBlockByHash`   | 通过哈希获取区块         | `[blockHash, fullTxObjects]`                | `object` - 区块对象 | 通过哈希查询区块    |
| `eth_call`             | 调用智能合约函数（只读） | `[{ to, data }, blockParameter]`            | `string` - 返回数据 | 不消耗Gas的合约调用 |
| `eth_getLogs`          | 获取事件日志             | `[{ fromBlock, toBlock, address, topics }]` | `array` - 日志数组  | 查询合约事件        |

## 加密相关方法 (Encryption Methods)

| 方法名                       | 作用         | 参数                          | 返回值                  | 备注           |
| ---------------------------- | ------------ | ----------------------------- | ----------------------- | -------------- |
| `eth_getEncryptionPublicKey` | 获取加密公钥 | `[address]`                   | `string` - 公钥         | 用于端到端加密 |
| `eth_decrypt`                | 解密消息     | `[encryptedMessage, address]` | `string` - 解密后的消息 | 解密加密消息   |

## MetaMask特有方法 (MetaMask-Specific Methods)

| 方法名                      | 作用         | 参数      | 返回值              | 备注                 |
| --------------------------- | ------------ | --------- | ------------------- | -------------------- |
| `wallet_registerOnboarding` | 注册引导流程 | 无        | `boolean`           | 引导用户安装MetaMask |
| `wallet_scanQRCode`         | 扫描二维码   | `[regex]` | `string` - 扫描结果 | 仅移动端可用         |

## 实验性/已弃用方法 (Experimental/Deprecated Methods)

| 方法名              | 作用                 | 参数 | 返回值                | 备注                         |
| ------------------- | -------------------- | ---- | --------------------- | ---------------------------- |
| `ethereum.enable()` | 连接钱包（已弃用）   | 无   | `string[]` - 账户数组 | 请使用 `eth_requestAccounts` |
| `eth_coinbase`      | 获取主账户（已弃用） | 无   | `string` - 地址       | 请使用 `eth_accounts`        |

## 使用示例

```javascript
// 连接钱包
const accounts = await window.ethereum.request({ 
  method: 'eth_requestAccounts' 
});

// 发送交易
const txHash = await window.ethereum.request({
  method: 'eth_sendTransaction',
  params: [{
    from: accounts[0],
    to: '0x...',
    value: '0x16345785d8a0000', // 0.1 ETH in wei
    gas: '0x5208', // 21000
    gasPrice: '0x9184e72a000' // 10 gwei
  }]
});

// 签名消息
const signature = await window.ethereum.request({
  method: 'personal_sign',
  params: ['Hello World', accounts[0]]
});

// 切换网络
await window.ethereum.request({
  method: 'wallet_switchEthereumChain',
  params: [{ chainId: '0x89' }] // Polygon
});
```

## 错误处理

所有方法都可能返回错误，常见错误码：

- `4001`: 用户拒绝请求
- `4100`: 未授权的方法
- `4200`: 不支持的方法
- `4900`: 钱包断开连接
- `4901`: 链断开连接

```javascript
try {
  const result = await window.ethereum.request({ method: 'eth_requestAccounts' });
} catch (error) {
  if (error.code === 4001) {
    console.log('用户拒绝了连接请求');
  } else {
    console.error('发生了错误:', error);
  }
}
```