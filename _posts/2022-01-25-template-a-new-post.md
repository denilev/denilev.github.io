---
title     : "Шаблон Нового Поста"
author    :
  name    : "Денис"
  link    : https://github.com/denilev
date      : 2022-01-19 14:10:00 +0800
categories: [Справка, Демо]
tags      : [шаблон]
---

Этот шаблон поста представлен здесь для наглядности заполнения. Со временем будет дополнен, а пока здесь находится тоько заголовок. В заголовке может быть обязательным заполнить название, остальные параметры в таком случае остануться как указано в настройках по умолчанию. 


```yaml
---
title: TITLE
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [Топ_Категория, Под_Категория]
tags: [Тег]                                # ТЕГИ всегда должны быть в нижнем регистре
pin: true                                  # закрепить пост вверху
image:
  src: /cdncontent/Bali.jpg                # титульная картинка из CDN хранилища
  width: 800
  height: 500
---
```


Чтобы добавить картинку в публикацию нужно залить её в репозиторий cdn, в папку pic, и как путь указать только её название + расширение.

```
https://cdn.jsdelivr.net/gh/denilev/cdn/pic/raven.jpg
https://cdn.jsdelivr.net/gh/GHuser/repo/folder/file.jpg
```
Чтобы обновить кеш CDN хранилища нужно заменить в ссылке `cdn` на `purge` как в примере ниже:
```
https://cdn.jsdelivr.net/gh/denilev/cdn/pic/raven.jpg
https://purge.jsdelivr.net/gh/denilev/cdn/pic/raven.jpg
```

### Пример языка Soliduty

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract AucEngine {
    address public owner;
    uint constant DURATION = 2 days; // 2 * 24 * 60 * 60
    uint constant FEE = 10; // 10%
    // immutable
    struct Auction {
        address payable seller;
        uint startingPrice;
        uint finalPrice;
        uint startAt;
        uint endsAt;
        uint discountRate;
        string item;
        bool stopped;
        
    }

    Auction[] public auctions;

    event AuctionCreated(uint index, string itemName, uint startingPrice, uint duration);
    event AuctionEnded(uint index, uint finalPrice, address winner);

    constructor() {
        owner = msg.sender;
    }
    modifier onlyOwner() {
        require();
        _;
    }
    function withdraw() external onlyOwner {
        //...
    }

    function createAuction(uint _startingPrice, uint _discountRate, string memory _item, uint _duration) external {
        uint duration = _duration == 0 ? DURATION : _duration;

        require(_startingPrice >= _discountRate * duration, "incorrect starting price");

        Auction memory newAuction = Auction({
            seller: payable(msg.sender),
            startingPrice: _startingPrice,
            finalPrice: _startingPrice,
            discountRate: _discountRate,
            startAt: block.timestamp, // now
            endsAt: block.timestamp + duration,
            item: _item,
            stopped: false
        });

        auctions.push(newAuction);

        emit AuctionCreated(auctions.length - 1, _item, _startingPrice, duration);
    }

    function getPriceFor(uint index) public view returns(uint) {
        Auction memory cAuction = auctions[index];
        require(!cAuction.stopped, "stopped!");
        uint elapsed = block.timestamp - cAuction.startAt;
        uint discount = cAuction.discountRate * elapsed;
        return cAuction.startingPrice - discount;
    }

    function buy(uint index) external payable {
        Auction storage cAuction = auctions[index];
        require(!cAuction.stopped, "stopped!");
        require(block.timestamp < cAuction.endsAt, "ended!");
        uint cPrice = getPriceFor(index);
        require(msg.value >= cPrice, "not enough funds!");
        cAuction.stopped = true;
        cAuction.finalPrice = cPrice;
        uint refund = msg.value - cPrice;
        if(refund > 0) {
            payable(msg.sender).transfer(refund);
        }
        cAuction.seller.transfer(
            cPrice - ((cPrice * FEE) / 100)
        ); // 500
        // 500 - ((500 * 10) / 100) = 500 - 50 = 450
        // Math.floor --> JS
        emit AuctionEnded(index, cPrice, msg.sender);
    }
}
```
