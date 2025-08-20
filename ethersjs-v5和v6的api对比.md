# Ethers.js v5 vs v6 API 详细对比表格

## 1. Provider 相关API

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|:----------|-----------|----------|----------|
| **创建Provider** |||||
| JSON-RPC Provider | `new ethers.providers.JsonRpcProvider(url, network)` | `new ethers.JsonRpcProvider(url, network, options)` | `url`: RPC端点URL<br>`network`: 网络配置(可选)<br>`options`: 连接选项(v6新增) | 移除providers命名空间，新增options参数 |
| Web3 Provider | `new ethers.providers.Web3Provider(externalProvider, network)` | `new ethers.BrowserProvider(externalProvider, network)` | `externalProvider`: 外部provider(如window.ethereum)<br>`network`: 网络配置(可选) | 重命名为BrowserProvider |
| Alchemy Provider | `new ethers.providers.AlchemyProvider(network, apiKey)` | `new ethers.AlchemyProvider(network, apiKey, options)` | `network`: 网络名称或配置<br>`apiKey`: Alchemy API密钥<br>`options`: 连接选项(v6新增) | 移除providers命名空间 |
| Infura Provider | `new ethers.providers.InfuraProvider(network, projectId, projectSecret)` | `new ethers.InfuraProvider(network, projectId, projectSecret)` | `network`: 网络名称或配置<br>`projectId`: Infura项目ID<br>`projectSecret`: 项目密钥(可选) | 移除providers命名空间 |
| WebSocket Provider | `new ethers.providers.WebSocketProvider(url, network)` | `new ethers.WebSocketProvider(url, network)` | `url`: WebSocket URL<br>`network`: 网络配置(可选) | 移除providers命名空间 |
| **Provider查询方法** |||||
| 获取余额 | `provider.getBalance(addressOrName, blockTag)` | `provider.getBalance(addressOrName, blockTag)` | `addressOrName`: 地址或ENS名称<br>`blockTag`: 区块标识("latest", "pending", 区块号等) | 无变化 |
| 获取交易数 | `provider.getTransactionCount(addressOrName, blockTag)` | `provider.getTransactionCount(addressOrName, blockTag)` | `addressOrName`: 地址或ENS名称<br>`blockTag`: 区块标识 | 无变化 |

## 2. Wallet 相关API

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **创建钱包** |
| 随机钱包 | `ethers.Wallet.createRandom(options)` | `ethers.Wallet.createRandom(provider)` | `options`: 钱包选项{extraEntropy, locale, path}<br>`provider`: Provider实例(v6) | v6简化了参数 |
| 从私钥 | `new ethers.Wallet(privateKey, provider)` | `new ethers.Wallet(privateKey, provider)` | `privateKey`: 私钥字符串或Uint8Array<br>`provider`: Provider实例(可选) | 无变化 |
| 从助记词 | `ethers.Wallet.fromMnemonic(mnemonic, path, wordlist)` | `ethers.Wallet.fromPhrase(mnemonic, provider, path, wordlist)` | `mnemonic`: 助记词字符串<br>`path`: 派生路径(默认"m/44'/60'/0'/0/0")<br>`wordlist`: 词汇表<br>`provider`: Provider实例(v6新增) | 方法名变更，参数顺序调整 |
| 从加密JSON | `ethers.Wallet.fromEncryptedJson(json, password, progress)` | `ethers.Wallet.fromEncryptedJson(json, password, progress)` | `json`: 加密的JSON字符串<br>`password`: 解密密码<br>`progress`: 进度回调函数(可选) | 无变化 |
| **钱包连接** |
| 连接Provider | `wallet.connect(provider)` | `wallet.connect(provider)` | `provider`: Provider实例 | 无变化 |
| **钱包属性** |
| 获取地址 | `wallet.address` | `wallet.address` | 只读属性 | 无变化 |
| 获取私钥 | `wallet.privateKey` | `wallet.privateKey` | 只读属性 | 无变化 |
| 获取公钥 | `wallet.publicKey` | `wallet.publicKey` | 只读属性 | 无变化 |
| **签名相关** |
| 签名消息 | `wallet.signMessage(message)` | `wallet.signMessage(message)` | `message`: 要签名的消息(字符串或Uint8Array) | 无变化 |
| 签名交易 | `wallet.signTransaction(transaction)` | `wallet.signTransaction(transaction)` | `transaction`: 交易对象{to, value, gasLimit, gasPrice等} | 无变化 |
| 签名类型化数据 | `wallet._signTypedData(domain, types, value)` | `wallet.signTypedData(domain, types, value)` | `domain`: EIP-712域<br>`types`: 类型定义<br>`value`: 要签名的值 | v6移除了下划线前缀 |
| **钱包加密** |
| 加密钱包 | `wallet.encrypt(password, options, progress)` | `wallet.encrypt(password, options, progress)` | `password`: 加密密码<br>`options`: 加密选项<br>`progress`: 进度回调(可选) | 无变化 |

## 3. Contract 相关API

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **创建合约** |
| 合约实例 | `new ethers.Contract(address, abi, signerOrProvider)` | `new ethers.Contract(address, abi, runner)` | `address`: 合约地址<br>`abi`: 合约ABI<br>`runner`: Provider或Signer实例 | signerOrProvider重命名为runner |
| 合约工厂 | `new ethers.ContractFactory(abi, bytecode, signer)` | `new ethers.ContractFactory(abi, bytecode, runner)` | `abi`: 合约ABI<br>`bytecode`: 合约字节码<br>`runner`: Signer实例 | signer重命名为runner |
| **合约部署** |
| 部署合约 | `contractFactory.deploy(...args, overrides)` | `contractFactory.deploy(...args, overrides)` | `...args`: 构造函数参数<br>`overrides`: 交易覆盖选项{gasLimit, gasPrice, value等} | 无变化 |
| 获取部署交易 | `contractFactory.getDeployTransaction(...args, overrides)` | `contractFactory.getDeployTransaction(...args, overrides)` | `...args`: 构造函数参数<br>`overrides`: 交易覆盖选项 | 无变化 |
| **合约交互** |
| 调用只读方法 | `contract.methodName(...args, overrides)` | `contract.methodName(...args, overrides)` | `...args`: 方法参数<br>`overrides`: 调用覆盖选项{blockTag} | 无变化 |
| 发送交易 | `contract.methodName(...args, overrides)` | `contract.methodName(...args, overrides)` | `...args`: 方法参数<br>`overrides`: 交易覆盖选项{value, gasLimit等} | 无变化 |
| 估算Gas | `contract.estimateGas.methodName(...args, overrides)` | `contract.methodName.estimateGas(...args, overrides)` | `...args`: 方法参数<br>`overrides`: 交易覆盖选项 | API调用方式变更 |
| 填充交易 | `contract.populateTransaction.methodName(...args, overrides)` | `contract.methodName.populateTransaction(...args, overrides)` | `...args`: 方法参数<br>`overrides`: 交易覆盖选项 | API调用方式变更 |
| **事件监听** |
| 监听事件 | `contract.on(eventFilter, listener)` | `contract.on(eventFilter, listener)` | `eventFilter`: 事件过滤器或事件名<br>`listener`: 事件回调函数 | 无变化 |
| 监听一次 | `contract.once(eventFilter, listener)` | `contract.once(eventFilter, listener)` | `eventFilter`: 事件过滤器或事件名<br>`listener`: 事件回调函数 | 无变化 |
| 移除监听 | `contract.off(eventFilter, listener)` | `contract.off(eventFilter, listener)` | `eventFilter`: 事件过滤器或事件名<br>`listener`: 要移除的回调函数 | 无变化 |
| 查询历史事件 | `contract.queryFilter(eventFilter, fromBlock, toBlock)` | `contract.queryFilter(eventFilter, fromBlock, toBlock)` | `eventFilter`: 事件过滤器<br>`fromBlock`: 起始区块<br>`toBlock`: 结束区块 | 无变化 |
| **合约属性** |
| 合约地址 | `contract.address` | `contract.target` | 只读属性 | 属性名从address改为target |
| 合约接口 | `contract.interface` | `contract.interface` | 只读属性 | 无变化 |
| 签名者/提供者 | `contract.signer / contract.provider` | `contract.runner` | 只读属性 | 合并为runner属性 |

## 4. Utils 工具函数

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **单位转换** |
| Wei转Ether | `ethers.utils.formatEther(value)` | `ethers.formatEther(value)` | `value`: Wei值(BigNumber/BigInt/string) | 移除utils命名空间 |
| Ether转Wei | `ethers.utils.parseEther(value)` | `ethers.parseEther(value)` | `value`: Ether值(string/number) | 移除utils命名空间 |
| 格式化单位 | `ethers.utils.formatUnits(value, unitName)` | `ethers.formatUnits(value, unit)` | `value`: 数值<br>`unit`: 单位名称或小数位数 | 移除utils命名空间 |
| 解析单位 | `ethers.utils.parseUnits(value, unitName)` | `ethers.parseUnits(value, unit)` | `value`: 数值字符串<br>`unit`: 单位名称或小数位数 | 移除utils命名空间 |
| **地址处理** |
| 地址校验 | `ethers.utils.isAddress(address)` | `ethers.isAddress(address)` | `address`: 要校验的地址字符串 | 移除utils命名空间 |
| 地址格式化 | `ethers.utils.getAddress(address)` | `ethers.getAddress(address)` | `address`: 地址字符串(任意大小写) | 移除utils命名空间 |
| 计算合约地址 | `ethers.utils.getContractAddress(transaction)` | `ethers.getContractAddress(transaction)` | `transaction`: 包含{from, nonce}的交易对象 | 移除utils命名空间 |
| CREATE2地址 | `ethers.utils.getCreate2Address(from, salt, initCodeHash)` | `ethers.getCreate2Address(from, salt, initCodeHash)` | `from`: 部署者地址<br>`salt`: 盐值<br>`initCodeHash`: 初始化代码哈希 | 移除utils命名空间 |
| **哈希函数** |
| Keccak256 | `ethers.utils.keccak256(data)` | `ethers.keccak256(data)` | `data`: 要哈希的数据(Uint8Array/string) | 移除utils命名空间 |
| SHA256 | `ethers.utils.sha256(data)` | `ethers.sha256(data)` | `data`: 要哈希的数据(Uint8Array/string) | 移除utils命名空间 |
| RIPEMD160 | `ethers.utils.ripemd160(data)` | `ethers.ripemd160(data)` | `data`: 要哈希的数据(Uint8Array/string) | 移除utils命名空间 |
| **字节处理** |
| 转换为字节 | `ethers.utils.toUtf8Bytes(str, form)` | `ethers.toUtf8Bytes(str, form)` | `str`: UTF-8字符串<br>`form`: 规范化形式(可选) | 移除utils命名空间 |
| 转换为字符串 | `ethers.utils.toUtf8String(bytes, onError)` | `ethers.toUtf8String(bytes, onError)` | `bytes`: 字节数组<br>`onError`: 错误处理函数(可选) | 移除utils命名空间 |
| 十六进制转换 | `ethers.utils.hexlify(value, options)` | `ethers.hexlify(value, width)` | `value`: 要转换的值<br>`width`: 固定宽度(v6) | 移除utils命名空间，参数简化 |
| 数组拼接 | `ethers.utils.concat(arrays)` | `ethers.concat(arrays)` | `arrays`: 字节数组列表 | 移除utils命名空间 |
| 数组切片 | `ethers.utils.hexDataSlice(data, offset, endOffset)` | `ethers.dataSlice(data, start, end)` | `data`: 十六进制数据<br>`start`: 起始位置<br>`end`: 结束位置 | 方法名和参数名变更 |
| **编码解码** |
| Base58编码 | `ethers.utils.base58.encode(data)` | `ethers.encodeBase58(data)` | `data`: 要编码的字节数据 | 简化API |
| Base58解码 | `ethers.utils.base58.decode(data)` | `ethers.decodeBase58(data)` | `data`: Base58字符串 | 简化API |
| Base64编码 | `ethers.utils.base64.encode(data)` | `ethers.encodeBase64(data)` | `data`: 要编码的字节数据 | 简化API |
| Base64解码 | `ethers.utils.base64.decode(data)` | `ethers.decodeBase64(data)` | `data`: Base64字符串 | 简化API |

## 5. BigNumber 处理

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **BigNumber创建** |
| 创建BigNumber | `ethers.BigNumber.from(value)` | `BigInt(value)` | `value`: 数值、字符串或十六进制 | 完全移除BigNumber类，使用原生BigInt |
| 最大值 | `ethers.BigNumber.from(a).max(b)` | `a > b ? a : b` | `a`, `b`: BigInt值 | 使用原生比较操作符 |
| 最小值 | `ethers.BigNumber.from(a).min(b)` | `a < b ? a : b` | `a`, `b`: BigInt值 | 使用原生比较操作符 |
| **数学运算** |
| 加法 | `bigNumber.add(other)` | `bigInt + otherBigInt` | `other`: 另一个BigNumber/BigInt | 使用原生BigInt操作符 |
| 减法 | `bigNumber.sub(other)` | `bigInt - otherBigInt` | `other`: 另一个BigNumber/BigInt | 使用原生BigInt操作符 |
| 乘法 | `bigNumber.mul(other)` | `bigInt * otherBigInt` | `other`: 另一个BigNumber/BigInt | 使用原生BigInt操作符 |
| 除法 | `bigNumber.div(other)` | `bigInt / otherBigInt` | `other`: 另一个BigNumber/BigInt | 使用原生BigInt操作符 |
| 取模 | `bigNumber.mod(other)` | `bigInt % otherBigInt` | `other`: 另一个BigNumber/BigInt | 使用原生BigInt操作符 |
| 幂运算 | `bigNumber.pow(exponent)` | `bigInt ** exponentBigInt` | `exponent`: 指数值 | 使用原生BigInt操作符 |
| **比较操作** |
| 相等 | `bigNumber.eq(other)` | `bigInt === otherBigInt` | `other`: 另一个BigNumber/BigInt | 使用原生比较操作符 |
| 大于 | `bigNumber.gt(other)` | `bigInt > otherBigInt` | `other`: 另一个BigNumber/BigInt | 使用原生比较操作符 |
| 小于 | `bigNumber.lt(other)` | `bigInt < otherBigInt` | `other`: 另一个BigNumber/BigInt | 使用原生比较操作符 |
| **转换方法** |
| 转字符串 | `bigNumber.toString()` | `bigInt.toString()` | 无参数 | 使用原生BigInt方法 |
| 转数字 | `bigNumber.toNumber()` | `Number(bigInt)` | 无参数 | 使用原生Number构造函数 |
| 转十六进制 | `bigNumber.toHexString()` | `"0x" + bigInt.toString(16)` | 无参数 | 手动拼接十六进制前缀 |

## 6. ABI 相关

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **ABI编码解码** |
| 获取ABI Coder | `ethers.utils.defaultAbiCoder` | `ethers.AbiCoder.defaultAbiCoder()` | 无参数 | 重构为类方法 |
| 编码数据 | `ethers.utils.defaultAbiCoder.encode(types, values)` | `AbiCoder.defaultAbiCoder().encode(types, values)` | `types`: 类型数组<br>`values`: 值数组 | API调用方式变更 |
| 解码数据 | `ethers.utils.defaultAbiCoder.decode(types, data)` | `AbiCoder.defaultAbiCoder().decode(types, data)` | `types`: 类型数组<br>`data`: 编码的十六进制数据 | API调用方式变更 |
| **接口处理** |
| 创建接口 | `new ethers.utils.Interface(abi)` | `new ethers.Interface(abi)` | `abi`: ABI数组或JSON字符串 | 移除utils命名空间 |
| 编码函数数据 | `interface.encodeFunctionData(fragment, values)` | `interface.encodeFunctionData(fragment, values)` | `fragment`: 函数名或Fragment对象<br>`values`: 参数值数组 | 无变化 |
| 解码函数结果 | `interface.decodeFunctionResult(fragment, data)` | `interface.decodeFunctionResult(fragment, data)` | `fragment`: 函数名或Fragment对象<br>`data`: 返回的十六进制数据 | 无变化 |
| 编码函数调用 | `interface.encodeFunctionCall(fragment, values)` | 已移除 | 已移除此方法 | v6中移除 |
| **事件处理** |
| 编码事件主题 | `interface.encodeEventLog(eventFragment, values)` | `interface.encodeEventLog(eventFragment, values)` | `eventFragment`: 事件Fragment<br>`values`: 事件参数值 | 无变化 |
| 解码事件日志 | `interface.decodeEventLog(eventFragment, data, topics)` | `interface.decodeEventLog(eventFragment, data, topics)` | `eventFragment`: 事件Fragment<br>`data`: 日志数据<br>`topics`: 主题数组 | 无变化 |
| 解析日志 | `interface.parseLog(log)` | `interface.parseLog(log)` | `log`: 日志对象{topics, data} | 无变化 |
| **错误处理** |
| 解码错误 | `interface.decodeErrorResult(fragment, data)` | `interface.decodeErrorResult(fragment, data)` | `fragment`: 错误Fragment<br>`data`: 错误数据 | 无变化 |
| 解析错误 | `interface.parseError(data)` | `interface.parseError(data)` | `data`: 错误的十六进制数据 | 无变化 |
| **Fragment操作** |
| 获取函数 | `interface.getFunction(nameOrSignature)` | `interface.getFunction(nameOrSignature)` | `nameOrSignature`: 函数名或签名 | 无变化 |
| 获取事件 | `interface.getEvent(nameOrSignature)` | `interface.getEvent(nameOrSignature)` | `nameOrSignature`: 事件名或签名 | 无变化 |
| 格式化函数 | `interface.format(format)` | `interface.format(format)` | `format`: 格式化选项("full", "minimal"等) | 无变化 |

## 7. 网络和配置

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **网络定义** |
| 获取网络 | `ethers.providers.getNetwork(network)` | `ethers.Network.from(network)` | `network`: 网络名称、链ID或网络对象 | API重构 |
| 创建网络 | `new ethers.providers.Network(name, chainId)` | `new ethers.Network(name, chainId)` | `name`: 网络名称<br>`chainId`: 链ID | 移除providers命名空间 |
| **常量定义** |
| 零地址 | `ethers.constants.AddressZero` | `ethers.ZeroAddress` | 常量值："0x0000000000000000000000000000000000000000" | 简化命名空间 |
| 零哈希 | `ethers.constants.HashZero` | `ethers.ZeroHash` | 常量值："0x0000000000000000000000000000000000000000000000000000000000000000" | 简化命名空间 |
| 最大Uint256 | `ethers.constants.MaxUint256` | `ethers.MaxUint256` | 常量值：2^256 - 1 | 简化命名空间 |
| 最大Int256 | `ethers.constants.MaxInt256` | `ethers.MaxInt256` | 常量值：2^255 - 1 | 简化命名空间 |
| 最小Int256 | `ethers.constants.MinInt256` | `ethers.MinInt256` | 常量值：-2^255 | 简化命名空间 |
| **ENS相关** |
| 解析ENS名称 | `provider.resolveName(name)` | `provider.resolveName(name)` | `name`: ENS域名字符串 | 无变化 |
| 反向解析 | `provider.lookupAddress(address)` | `provider.lookupAddress(address)` | `address`: 以太坊地址 | 无变化 |

## 8. 错误处理和日志

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **错误类型** |
| 通用错误 | `ethers.errors.*` | `ethers.*Error` | 各种错误类型 | 错误类型重新组织 |
| **日志设置** |
| 设置日志级别 | `ethers.utils.Logger.setLogLevel(level)` | 使用console或自定义日志 | `level`: 日志级别 | v6移除了内置日志系统 |

## 9. 主要迁移变化总结

### 🔄 命名空间变化
- **移除**: `ethers.providers.*` → `ethers.*`
- **移除**: `ethers.utils.*` → `ethers.*`
- **重命名**: `Web3Provider` → `BrowserProvider`

### 🔧 BigNumber → BigInt
- **v5**: 自定义的`BigNumber`类，方法调用形式
- **v6**: 原生JavaScript的`BigInt`，操作符形式

### 📝 方法重命名
| v5 | v6 | 参数变化 |
|----|----|----|
| `Wallet.fromMnemonic(mnemonic, path, wordlist)` | `Wallet.fromPhrase(mnemonic, provider, path, wordlist)` | 新增provider参数 |
| `provider.sendTransaction(tx)` | `provider.broadcastTransaction(tx)` | 无参数变化 |
| `provider.getGasPrice()` | `provider.getFeeData()` | 返回值结构变化 |
| `contract.address` | `contract.target` | 属性名变化 |

### 🎯 API调用方式变化
| v5 | v6 |
|----|----|
| `contract.estimateGas.method()` | `contract.method.estimateGas()` |
| `contract.populateTransaction.method()` | `contract.method.populateTransaction()` |
| `ethers.utils.defaultAbiCoder` | `ethers.AbiCoder.defaultAbiCoder()` |

### ⚡ 性能和体积优化
- **包体积**: v6显著减小，更好的Tree-shaking
- **执行速度**: v6使用原生BigInt，性能提升
- **内存使用**: v6优化了内存管理

### 🔒 TypeScript改进
- **类型推断**: v6提供更准确的类型推断
- **类型安全**: 更严格的类型检查
- **智能提示**: 更好的IDE支持

## 10. 高级功能API

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **HD钱包** |
| 创建HD节点 | `ethers.utils.HDNode.fromMnemonic(mnemonic, password, wordlist)` | `ethers.HDNodeWallet.fromPhrase(mnemonic, password, path, wordlist)` | `mnemonic`: 助记词<br>`password`: 可选密码<br>`path`: 派生路径<br>`wordlist`: 词汇表 | 类名和方法名变更 |
| 派生子节点 | `hdnode.derivePath(path)` | `hdwallet.derivePath(path)` | `path`: 派生路径字符串 | 无变化 |
| 派生子钱包 | `hdnode.derive(index)` | `hdwallet.deriveChild(index)` | `index`: 子索引号 | 方法名变更 |
| **签名验证** |
| 恢复签名者 | `ethers.utils.verifyMessage(message, signature)` | `ethers.verifyMessage(message, signature)` | `message`: 原始消息<br>`signature`: 签名字符串 | 移除utils命名空间 |
| 恢复类型化签名 | `ethers.utils.verifyTypedData(domain, types, value, signature)` | `ethers.verifyTypedData(domain, types, value, signature)` | `domain`: EIP-712域<br>`types`: 类型定义<br>`value`: 签名值<br>`signature`: 签名 | 移除utils命名空间 |
| **密码学函数** |
| 计算公钥 | `ethers.utils.computePublicKey(privateKey, compressed)` | `ethers.computePublicKey(privateKey, compressed)` | `privateKey`: 私钥<br>`compressed`: 是否压缩公钥 | 移除utils命名空间 |
| ECDH共享密钥 | `ethers.utils.computeSharedSecret(privateKey, publicKey)` | `ethers.computeSharedSecret(privateKey, publicKey)` | `privateKey`: 自己的私钥<br>`publicKey`: 对方的公钥 | 移除utils命名空间 |
| **随机数生成** |
| 安全随机字节 | `ethers.utils.randomBytes(length)` | `ethers.randomBytes(length)` | `length`: 字节长度 | 移除utils命名空间 |
| **网络请求** |
| 发送HTTP请求 | `ethers.utils.fetchJson(url, json, processFunc)` | 使用原生fetch或第三方库 | `url`: 请求URL<br>`json`: 请求体<br>`processFunc`: 处理函数 | v6移除了内置HTTP客户端 |

## 11. 交易相关API

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **交易构建** |
| 创建交易 | `{to, value, gasLimit, gasPrice, nonce, data}` | `{to, value, gasLimit, gasPrice, maxFeePerGas, maxPriorityFeePerGas, nonce, data, type}` | `to`: 接收地址<br>`value`: 转账金额<br>`gasLimit`: Gas限制<br>`gasPrice`: Gas价格(Legacy)<br>`maxFeePerGas`: 最大费用(EIP-1559)<br>`maxPriorityFeePerGas`: 优先费用(EIP-1559)<br>`nonce`: 交易序号<br>`data`: 交易数据<br>`type`: 交易类型 | v6增强EIP-1559支持 |
| **交易序列化** |
| 序列化交易 | `ethers.utils.serializeTransaction(transaction, signature)` | `ethers.Transaction.serialized` | `transaction`: 交易对象<br>`signature`: 签名对象 | API重构 |
| 解析交易 | `ethers.utils.parseTransaction(rawTransaction)` | `ethers.Transaction.from(rawTransaction)` | `rawTransaction`: 原始交易数据 | API重构 |
| **费用估算** |
| 估算Gas | `provider.estimateGas(transaction)` | `provider.estimateGas(transaction)` | `transaction`: 交易对象 | 无变化 |
| 获取费用数据 | `provider.getGasPrice()` | `provider.getFeeData()` | 无参数 | v6返回EIP-1559费用结构 |

## 12. 事件和过滤器API

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **事件过滤器** |
| 创建过滤器 | `contract.filters.EventName(...args)` | `contract.filters.EventName(...args)` | `...args`: 索引参数，null表示任意值 | 无变化 |
| 主题过滤器 | `{address, topics: [topic0, topic1, topic2]}` | `{address, topics: [topic0, topic1, topic2]}` | `address`: 合约地址<br>`topics`: 主题数组 | 无变化 |
| **事件查询** |
| 查询历史事件 | `provider.getLogs({fromBlock, toBlock, address, topics})` | `provider.getLogs({fromBlock, toBlock, address, topics})` | `fromBlock`: 起始区块<br>`toBlock`: 结束区块<br>`address`: 合约地址<br>`topics`: 主题过滤器 | 无变化 |
| **实时监听** |
| 监听新区块 | `provider.on("block", blockNumber => {})` | `provider.on("block", blockNumber => {})` | 回调参数：`blockNumber`: 新区块号 | 无变化 |
| 监听pending交易 | `provider.on("pending", txHash => {})` | `provider.on("pending", txHash => {})` | 回调参数：`txHash`: 交易哈希 | 无变化 |

## 13. 错误处理和调试

| 功能分类 | Ethers v5 | Ethers v6 | 参数说明 | 主要变化 |
|---------|-----------|-----------|----------|----------|
| **错误类型** |
| 网络错误 | `ethers.errors.NETWORK_ERROR` | `ethers.NetworkError` | 网络连接相关错误 | 错误类型重构 |
| 交易失败 | `ethers.errors.CALL_EXCEPTION` | `ethers.CallExceptionError` | 合约调用失败错误 | 错误类型重构 |
| 参数错误 | `ethers.errors.INVALID_ARGUMENT` | `ethers.InvalidArgumentError` | 参数验证失败错误 | 错误类型重构 |
| **调试信息** |
| 交易回执 | `receipt.logs`, `receipt.events` | `receipt.logs` | `logs`: 原始日志数组 | v6移除了events属性 |
| Gas使用 | `receipt.gasUsed` | `receipt.gasUsed` | 实际使用的Gas量 | 无变化 |
| 交易状态 | `receipt.status` | `receipt.status` | 1表示成功，0表示失败 | 无变化 |

## 14. 实用示例对比

### 连接钱包示例
```javascript
// v5
const provider = new ethers.providers.Web3Provider(window.ethereum);
const signer = provider.getSigner();
const balance = await provider.getBalance(await signer.getAddress());
const etherBalance = ethers.utils.formatEther(balance);

// v6
const provider = new ethers.BrowserProvider(window.ethereum);
const signer = await provider.getSigner();
const balance = await provider.getBalance(await signer.getAddress());
const etherBalance = ethers.formatEther(balance);
```

### 合约交互示例
```javascript
// v5
const contract = new ethers.Contract(address, abi, signer);
const gasEstimate = await contract.estimateGas.transfer(to, amount);
const tx = await contract.transfer(to, amount, {gasLimit: gasEstimate});

// v6
const contract = new ethers.Contract(address, abi, signer);
const gasEstimate = await contract.transfer.estimateGas(to, amount);
const tx = await contract.transfer(to, amount, {gasLimit: gasEstimate});
```

### BigNumber处理示例
```javascript
// v5
const amount = ethers.BigNumber.from("1000000000000000000");
const doubled = amount.mul(2);
const isEqual = amount.eq(ethers.utils.parseEther("1"));

// v6
const amount = BigInt("1000000000000000000");
const doubled = amount * 2n;
const isEqual = amount === ethers.parseEther("1");
```

