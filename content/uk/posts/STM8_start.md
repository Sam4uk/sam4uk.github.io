---
title: STM8
date: 2024-01-15
publish : 2024-01-15
lastmod: 2024-01-17T00:15:00
description: Мій перший восьми бітний STM
draft: false
hideToc: false
enableToc: true
enableTocContent: true
# author: Choi
authoravatar:
authorEmoji: 🤖
tags:
- C
- C
categories:
- Microchip
- Atmel
- ATMega
series:
- STM
image: https://sisoog.com/wp-content/uploads/2020/04/STM8-logo-1.png
---

# Ще один 8 бітний вечір

Нині важко когось здивувати тим що ти вмієш поводитись з контролерами `AVR`. Курсів, що пропонують вивчити `Arduino` стільки що всі й не порахувати. Пропонуються різні модифікації плат. Але їх поєднує одне... ЇХ називають просто `Arduino`. 

Вже навіть дітей вчать програмувати на цій платі...

А на ринку праці вимагають, щоб розробник знався з кристалами `STM`.

Сьогодні вечір дорослішання.

## Підготовка

Для програмування нам потрібно: плата, комп'ютер, та копілятор.

Почну я з останнього. З копілятора... А згодом пошукаю у коробці "естемку"

### SDCC

Для компіляції програм для цього камінця я буду(так в мережі рекомендують) використовувати компілятор `sdcc` (__Small Device C Compiler__). Це колекція компіляторів для мови `ANSI C` для великої кількості архітектур одна з яких восьмимітний STM. Є версії для різних ОСей. Я буду використовувати версію для `linux`. Тож завантажуємо архів з необхідною версією з [http://sdcc.sourceforge.net/](http://sdcc.sourceforge.net/)

Відкриваємо термінал вводимо наступні команди

```zsh
mkdir  tmp 
cd tmp
tar xjf ../sdcc-4.4.0-rc2-amd64-unknown-linux2.5.tar.bz2
```

В результаті таких дій ми отримаємо розпаковані файли. В мене розпакований каталог з іменем `sdcc-4.4.0-rc2`, а у вас він може відрізнятися. Крокуємо далі...

```zsh
cd sdcc-4.4.0-rc2
ls
```

Ми перейшли до каталогу з компіляторами які скачали

```zsh
rm *.txt
```

Видаляємо зайві файли, які в системі не потрібні. Ви ж не забули проглянути вміст файлів які ми тільки що видалили? Ні. Пізно :smile:...

Продовжуємо інсталяцію.

Важливо!!! Тепер нам знадобляться права суперкористувача. Якщо нам потрібно виконати команду від імені `root`:

```zsh
sudo cp -r * /usr/local
```

І вводимо пароль

### Перевіримо чи працює

Тепер перевіримо чи працює

```zsh
/usr/local/bin/sdcc -v 
```

Програма відгукнулась мені
```
SDCC : mcs51/z80/z180/r2k/r2ka/r3ka/sm83/tlcs90/ez80_z80/z80n/r800/ds390/pic16/pic14/TININative/ds400/hc08/s08/stm8/pdk13/pdk14/pdk15/mos6502/mos65c02 TD- 4.4.0 #14579 (Linux)
published under GNU General Public License (GPL)
```
ми бачимо перелік всього що ми можемо запрограмувати цим компілятором

Ще один тест. Кличу прогу без повного шляху

```zsh
sdcc -v
```

Програма успішно відгукується так само

## Пограємось з компілятором

Хочу погратись з компілятором таким чином. Подразнити його. В якому форматі він мене матюкати буде?

Створюю порожній файл `stm8.cpp` і згодовою його компілятору

```zsh
sdcc stm.cpp
```

Отримую перший матюк `error 119: don't know what to do with file 'stm.cpp'. file extension unsupported`. Ми не можемо програмувати мовю `C++`. (Мій наставник говорив: "Якщо знаєш С++, ти автоматично знаєш чистий Сі. Віримо і йдемо далі.)

Створємо `c`-файл і повторюємо. Посилаємо новий файл компілятору. Отримуємо нові повідомлення:
```
stm.c:1: warning 190: ISO C forbids an empty translation unit
stm.c:1: error 74: function 'main' undefined
```

Змінюю файл `stm.c` надаючи такого вигляду
```c
void main(void){};
```
<!--```
syntax error: token -> ';' ; column 18
```

```c
int main(){return 0;}
```
```
warning 283: function declarator with no prototype
```

```c
int main(void){return 0;}
```
Після чергового згодовування файлу компілятору він нас не обматюкав і згенерував багато файлів. Можна діставати плату з "бардачка" 
-->

## Знайомство з платою

### STM8L-DISCOVERY

Я колись давно придбав собі плату `STM8L-DISCOVERY` з якої хотів почати своє самостійне знайомство з контролерами `STM`. Нарешті настав її час.

`STM8L-DISCOVERY` зібрана на базі мікроконтролера `STM8L152C6T6` і має вбудований інтерфейс відлагодження `ST-Link`, рідкокристалічний дисплей, користувацьку кнопку та світлодіод. 

![](https://www.st.com/bin/ecommerce/api/image.PF250636.en.feature-description-include-personalized-no-cpn-large.jpg)

Сам мікроконтролер наділений `32KB` Flash, `2KB` оперативної пам'яті та `1KB` енергонезалежної пам'яті. Може працювати при напрузі джерала живлення від `1.8V`  до  `3.6V` працюючи на частоті `16MHz` і маючи при цьому 5 варіантів роботи в режимі енергозбереження. Виробники запхнули в нього 12 бітний цифроан-алоговий перетворювач та 12 бітний 25 канальний аналогово-цифровий перетворювач з можливістю робити `1M` замірів за секунду.

Про всі інші можливості можна дізнатися з даташиту на мікроконтреллери [STM8L151x4, STM8L151x6,STM8L152x4, STM8L152x6](https://www.st.com/content/ccc/resource/technical/document/datasheet/43/12/db/4c/8b/08/4a/73/CD00240181.pdf/files/CD00240181.pdf/jcr:content/translations/en.CD00240181.pdf)

Ось ще корисна інформація про плату розробки
 1. [STM8L ultralow power Discovery](https://www.st.com/resource/en/data_brief/stm8l-discovery.pdf)
 2. [STM8L-DISCOVERY](https://www.st.com/resource/en/user_manual/um0970-stm8ldiscovery-stmicroelectronics.pdf)
 3. [STM8L-DISCOVERY schematics](https://www.st.com/resource/en/schematic_pack/stm8l-discovery_sch.zip)
 4. [StdPeriph_Driver](https://www.st.com/content/ccc/resource/technical/software/firmware/fa/a9/6b/87/95/c2/4a/b4/stsw-stm8016.zip/files/stsw-stm8016.zip/jcr:content/translations/en.stsw-stm8016.zip)
 5. [st.com](https://www.st.com/en/evaluation-tools/stm8l-discovery.html)