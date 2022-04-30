---
title: "Шаблон Нового Поста"
date: 2022-01-19 14:10:00 +0800
categories: [Справка, Демо]
tags: [шаблон]
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
