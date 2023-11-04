---
title: Markdown синтаксис
date: 2019-12-20T12:00:06+09:00
publish : 2022-06-12
description: Проста демостіття демонстрація Markdown синтаксису та форматування для HTML елемнтів.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
# author: Choi
authoravatar:
authorEmoji: 🤖
tags:
- markdown
- css
- html
- themes
categories:
- themes
- syntax
series:
- Themes Guide
image: images/feature1/markdown.png
---

Ця стаття пропонує оглянути базові елементи синтаксису які можна використовувати в `Hugo` файлах контенту, також демонструє базові HTML елемненти які оформлені каскадними таблицями стилів встановленої теми.
<!--more-->

## Заголовки

Звичайний HTML підтримує 6 рівнів заголовків які позначають від `<h1>` до `<h6>`. Найстарший заголовок має рівень 1, а найшижчий 6.

# H1
## H2
### H3
#### H4
##### H5
###### H6
``` md
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

## Абзац

Кожен параграф починається відокремлюється від іншого порожнім рядком.

```md
Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur? Quianimin porecus evelectur, cum que nis nust voloribus ratem aut omnimi, sitatur? Quiatem. Nam, omnis sum am facea corem alique molestrunt et eos evelece arcillit ut aut eos eos nus, sin conecerem erum fuga. Ri oditatquam, ad quibus unda veliamenimin cusam et facea ipsamus es exerum sitate dolores editium rerore eost, temped molorro ratiae volorro te reribus dolorer sperchicium faceata tiustia prat.

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.
```

Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur? Quianimin porecus evelectur, cum que nis nust voloribus ratem aut omnimi, sitatur? Quiatem. Nam, omnis sum am facea corem alique molestrunt et eos evelece arcillit ut aut eos eos nus, sin conecerem erum fuga. Ri oditatquam, ad quibus unda veliamenimin cusam et facea ipsamus es exerum sitate dolores editium rerore eost, temped molorro ratiae volorro te reribus dolorer sperchicium faceata tiustia prat.

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.

## Цитата

Елемент цитати представляє вміст, який цитується з іншого джерела, необов'язково із цитуванням, яке повинно бути в межах `footer` або `cite` елемент і необов'язково із змінними змінами, такими як анотації та абревіатури.

#### Блокцитата без атребутів attribution

```md
> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Примітка** Ви можете використовувати *Markdown синтаксис* В блокцитаті.
```

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Примітка** Ви можете використовувати *Markdown синтаксис* В блокцитаті.

#### Блокцитата з атребутами

```md
> Don't communicate by sharing memory, share memory by communicating.</p>
> — <cite>Rob Pike[^1]</cite>


[^1]: Вищезазначена цитата витягнута з Rob Pike's [розмова](https://www.youtube.com/watch?v=PAAkCSZUG1c) під час Gopherfest, 18 листопада 2015 року.
```

Сноску шукайте в низу даної сторінки :smile:

> Don't communicate by sharing memory, share memory by communicating.</p>
> — <cite>Rob Pike[^1]</cite>


[^1]: Вищезазначена цитата витягнута з Rob Pike's [розмова](https://www.youtube.com/watch?v=PAAkCSZUG1c) під час Gopherfest, 18 листопада 2015 року.

## Таблиці

Таблиці не є частиною MarkDowm специфікації, але `Hugo` підтримує їх з коробки
```md
   Name | Age
--------|------
    Bob | 27
  Alice | 23
```
матиме такий вигляд

   Name | Age
--------|------
    Bob | 27
  Alice | 23

Таблицю можна такж "намалювати" таким способом
```md
|   Name | Age  |
|--------|------|
|    Bob | 27   |
|  Alice | 23   |
```
Обидва варіанти абсолютно тотожні.

Вирівнювання в таблицях задається таким чином

```md
| За замовчуванням | Ліворуч | Праворуч | Відцентровано |
|------------------|:--------|---------:|:-------------:|
|  звичайний текст | лівий   |  правий  |   середина    |
```

| За замовчуванням | Ліворуч | Праворуч | Відцентровано |
|------------------|:--------|---------:|:-------------:|
|  звичайний текст | лівий   |  правий  |   середина    |

#### Markdown в таблицях

Вбудовані у MarkDown таблиці мають наступний синтаксис
```markdown
|  Inline | Markdown | In                | Table  |
| ------- | -------- | ----------------- | ------ |
|*italics*| **bold** | ~~strikethrough~~ | `code` |
```
матимуть вигляд

|  Inline | Markdown | In                | Table  |
| ------- | -------- | ----------------- | ------ |
|*italics*| **bold** | ~~strikethrough~~ | `code` |

<!--
## Кодові блоки

#### Код блок із зворотнім апострофом

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
```
#### Код блоку відбудеться чотирма пробілами

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Example HTML5 Document</title>
    </head>
    <body>
      <p>Test</p>
    </body>
    </html>

#### Code block with Hugo's internal highlight shortcode

{{< highlight html >}}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
{{< /highlight >}}
-->

## Типи списків

#### Нумерований список

```md
1. First item
3. Second item
2. Third item
```
1. First item
3. Second item
2. Third item

> ***ПРИМІТКА*** Порядок нумерації не важливий. Важливо щоб починався з `1`

#### Не упорядкований список

```md

* List item
* Another item
* And another item
```

* List item
* Another item
* And another item

#### Вкладений список

```md
* Item
  1. First Sub-item
  2. Second Sub-item
```

* Item
  1. First Sub-item
  2. Second Sub-item

## Інші елементи — abbr, sub, sup, kbd, mark
```md
<abbr title="Graphics Interchange Format">GIF</abbr> це формат растровго зображення.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup>: Z<sup>n</sup>

Натисни <kbd><kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Delete</kbd></kbd>.
```
<abbr title="Graphics Interchange Format">GIF</abbr> це формат растровго зображення.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup>: Z<sup>n</sup>

Натисни <kbd><kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Delete</kbd></kbd>.

<!--
Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.-->

## HTML

Якщо вам бракує можливостей MarkDown ви можете використати `html` теги.

