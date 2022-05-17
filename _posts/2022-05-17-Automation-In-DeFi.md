---
title: "Python. DeFi автоматизация цепочек процессов"
author:
  name: "Денис"
  link: https://t.me/PolygonDevRussia/4843
date: 2022-05-17 23:26:00 +0800
categories: [Блокчейн, Python]
tags: [Автоматизация]
image:
  src: python.jpg
  width: 800
  height: 500
---

Автматизировать и ускорить, задать исполнение по расписанию любому процессу в блокчейне. Любые кнопки нажимаются сами. Быстро купить токен на самом старте продаж. .env Добавляю здесь новый код постоянно с практикой.

## Общее описание исходного фрагмента кода.

Готовим среду для последующего взаимодействия


1. подключаем библиотеки, которые понадобятся
2. явно указываем что будем работать с полигоном 
(способ получения информации из блокчейна и записи в него)
3. аккаунт 
приват ключ-
из приватного создаём публичный
сохраняем в переменную для удобства использования
4. принт пишет тру при успехе подключения  

Где посмотреть в эксплорере функцию транзакции (что происходит) :
клик на хеш, вкладка Overview, Click to see More, Input Data:

ApeSwap
Function: swapExactTokensForETH(uint256 amountIn, uint256 amountOutMin, address[] path, address to, uint256 deadline) ***

QuickSwap:
Function: swapExactETHForTokens(uint256 amountOutMin, address[] path, address to, uint256 deadline)

```python
from web3 import Web3
import json
import time

import os
import settings # dotenv

polygon_url = "https://rpc-mumbai.matic.today/"
web3 = Web3(Web3.HTTPProvider(polygon_url))

privateKey = os.getenv("PRIVATE_KEY")
account = web3.eth.account.privateKeyToAccount(privateKey)
publicKey = account.address
web3.eth.defaultAccount = account.address

print(web3.isConnected())
while True: # зациклить операцию бесконечно - внизу таймер
  nonce = web3.eth.getTransactionCount(web3.eth.defaultAccount)
  myData = '0xd204c45e0000000000000000000000007fee397e3058abced3e4ffa9be84f25506b8964d0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000005a68747470733a2f2f676174657761792e70696e6174612e636c6f75642f697066732f516d653857564b795361374e4574555178654353515a774e695a314d72525273636945764b6e7679585a785274702f4173616d692e6a7067000000000000'
  address = Web3.toChecksumAddress('0x1C1E0d77dcA2882628545D40aEea86Eb5C3A42D9')

  tx = {
    'from'  : publicKey,
    'nonce' : nonce,
    'gasPrice' : web3.toWei(35, 'gwei'), # GAS: Standard_31.4(Gwei), Fast_36(Gwei) https://polygonscan.com/gastracker
    'gas'  : 1500000,
    'to' : address,
    'data' : myData,
    'chainId': 80001 # в настройке сети ID цепочки
  }

  signed_tx = web3.eth.account.sign_transaction(tx, privateKey)
  tx_hash = web3.eth.send_raw_transaction(signed_tx.rawTransaction)
  time.sleep(10*3) # таймаут в цикле, первое число секунды, умноженное на что-то, (10*3)-это 30 секунд.
```

### settings.py

```python
# settings.py
## importing the load_dotenv from the python-dotenv module
from dotenv import load_dotenv

## using existing module to specify location of the .env file
from pathlib import Path

load_dotenv()
env_path = Path('.')/'.env'
load_dotenv(dotenv_path=env_path)

# retrieving keys and adding them to the project
# from the .env file through their key names
# PRIVATE_KEY = os.getenv("PRIVATE_KEY") - пример использования в моём коде 
```

### .env файл

```python
PRIVATE_KEY=*******************************
```

