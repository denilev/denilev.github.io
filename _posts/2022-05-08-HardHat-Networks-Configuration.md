---
title: "Настройка сетей для HardHat. Polygon, Rinkeby, Mumbai, ..."
author:
  name: "Денис"
  link: https://github.com/denilev
date: 2022-05-08 21:50:00 +0300
categories: [Solidity]
tags: [рабочая среда]
---

Копии файлов HardHat `hardhat.config.ts` для быстрой настройки различных сетей. +Основные комманды консоли для компиляции, деплоя, верификации, очистки.

### Mumbai

hardhat.config.ts

```typescript
import * as dotenv from 'dotenv'
import { HardhatUserConfig } from 'hardhat/config'

import '@nomiclabs/hardhat-etherscan'
import '@nomiclabs/hardhat-waffle'
import '@typechain/hardhat'
import 'hardhat-gas-reporter'
import 'solidity-coverage'

dotenv.config()

const config: HardhatUserConfig = {
  solidity: '0.8.4',
  networks: {
    polygon: {
      url: 'https://rpc-mumbai.maticvigil.com/',
      accounts:
        process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : []
    }
  },
  etherscan: {
    apiKey: {
      polygonMumbai: process.env.POLYGONSCAN_API_KEY
    }
  }
}

export default config
```

### Rinkeby

```typescript
import * as dotenv from 'dotenv'
import { HardhatUserConfig } from 'hardhat/config'

import '@nomiclabs/hardhat-etherscan'
import '@nomiclabs/hardhat-waffle'
import '@typechain/hardhat'
import 'hardhat-gas-reporter'
import 'solidity-coverage'

dotenv.config()

const config: HardhatUserConfig = {
  solidity: '0.8.4',
  networks: {
    rinkeby: {
      url: process.env.ALCHEMY_KEY,
      accounts:
        process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : []
    }
  },
  etherscan: {
    apiKey: process.env.ETHERSCAN_API_KEY
  }
}

export default config
```

```env
ALCHEMY_KEY=https://eth-rinkeby.alchemyapi.io/v2/ключ из сайта alchemy
ETHERSCAN_API_KEY=ключ для езерскана может быть не обязательным
PRIVATE_KEY=нужен обязательно для верификации
```

### Основные комманды HardHat
очистить старые компилированные версии
```
npx hardhat clean
```
компилировать файл-контракт .sol
```
npx hardhat compile
```

деплой смартконтракта
```
npx hardhat run scripts/deploy.ts --network rinkeby
```

верификация смартконтракта
```
npx hardhat verify --network mainnet DEPLOYED_CONTRACT_ADDRESS
```

Верификация в командной строке с передачей аргументов:
```shell
npx hardhat verify --network rinkeby 0x92B7141527ce02576446c922e6a1bA176B199C88 Mahoori MHR ipfs://QmeJ88g5bir68WAYBE6qxgj3f3JEmjhrKhmbohR5Qc6aSy/
```
