---
title: STM8
date: 2024-01-15
publish : 2024-01-15
lastmod: 2024-01-19T00:15:00
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

#### Заванатажуємо та встановлюємо

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

#### Перевіримо чи працює

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

#### Пограємось з компілятором

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
А ще карще почитаємо [інструкцію](https://sdcc.sourceforge.net/doc/sdccman.pdf)

### STM8FLASH

#### Завантажуємо та встановлюємо

Для завантаження програми на мікроконтроллер нам знадобиться окрім програматора утиліта `stm8flash`. Для операційних систем віндовс є готові виконувані файли цієї утиліти. А я на шастя не користутюсь операційною системою віндовс. Я буду збирати цю утиліту з джерельного коду, який доступний на `GitHub` за посиланням [https://github.com/vdudouyt/stm8flash](https://github.com/vdudouyt/stm8flash)

Для збирання за моєю інструкцією потрібно виконати наступні дії

```zsh
sudo apt-get update
sudo apt-get install -y pkg-config libusb-1.0-0-dev git make
mkdir ~/stm8_flash
cd ~/stm8_flash
git clone git@github.com:vdudouyt/stm8flash.git
cd stm8flash
make
sudo make install
```

#### Коротока перевірка

```zsh
stm8flash -V
```
В мене програма відгукнулас так:
```
20170616-1.1
```
<!--
### CMAKE

Мені дуже подобається система збірки `cmake`. спробуємо її прикрутити для роботи з великими та маленькими проектами 

#### Встановлюємо

```zsh
sudo apt-get install -y cmake
```

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
   
### Pin-control

Основана робота з портами введення виведння ведеться через п'ять регістрів на кожен порт. Ось вони
 - `Px_ODR` - _Port x output data register_. Регістр для виведення даних(аналог PORTx у AVRках). Якщо порт сконфігкрований на вихід, то запис в цей порт змінить електичний стан відповідної ніжки чіпа
 - `Px_IDR` - _Port x input data register_. Регістр читання даних (PINx у AVRках)
 - `Px_DDR` - _Port x data direcrion register_. Регістр визначає напрямок роботи порту. При низькому рівні в цьому регістрі, відповідний пін налаштований на вхід.
 - `Px_CR1` - _Port x control register 1_. Електричні властивості порту. Якщо пін налаштований на вхід запис `1` в цей регіст вмикає `pull_up` резистор в середині чіпа інакше вивід отримує `Z` стан (плаваючий). Якщо пін налаштваний на вихід то `1` в цьому регістрі вмикає `push-pull` режим, інакше ніжка працює як відкритий колектор.
 - `Px_CR2` - _Port x control register 2_. Якщо пін налаштовано на вхід `1` дозволяє генерацію зовнішнього переривання на піні, інакше таке заборонено. Якщо пін працює на виввід то запис `1` обмежує швидкість перемикання піна до 10 MHz, інакше - 2 MHz.
  
#### Дрик-Дрик ножкою

Спробуємо подригати ніжкою мікрокоторлеру.

В нас на платі є два користувацькі світлодіоди які через резистори піж'єднані до портів `PE7` та `PC7`. Спробуємо увімкнути їх.


```c
// Для потру C
#define PC_ODR    (*(volatile unsigned char *)0x500A)
#define PC_DDR    (*(volatile unsigned char *)0x500C)
#define PC_CR1    (*(volatile unsigned char *)0x500D)

// Для порту E
#define PE_ODR    (*(volatile unsigned char *)0x5014)
#define PE_DDR    (*(volatile unsigned char *)0x5016)
#define PE_CR1    (*(volatile unsigned char *)0x5017)

void main(void){
    PC_DDR=0b10000000; // на вихід
    PC_CR1=0b10000000; // пуш-пул
    PC_ODR=0b10000000; // подаємо сигнал
    PE_DDR=0b10000000;
    PE_CR1=0b10000000;
    PE_ODR=0b10000000;
    for(;;);// Більше нічого не робимо
}
```

Компілюємо  файл
```zsh
sdcc -mstm8 --std-c23 ./stm8.c  
```

Запихаємо у нашу плату
```zsh
stm8flash -c stlinkv2 -p stm8l152c6t6 -w stm8.ihx
```

Урра! Наші світлодіоди горять! Тобто світяться :smile:

А як щодо помигати світлодіодами?

Препишемо трошки код

```c
#define PC_ODR    (*(volatile unsigned char *)0x500A)
#define PC_DDR    (*(volatile unsigned char *)0x500C)
#define PC_CR1    (*(volatile unsigned char *)0x500D)

#define PE_ODR    (*(volatile unsigned char *)0x5014)
#define PE_DDR    (*(volatile unsigned char *)0x5016)
#define PE_CR1    (*(volatile unsigned char *)0x5017)

const auto DELAY=0xFFFF;

void main(void){
    // конфігуруємо порти
    PC_DDR=0b10000000; 
    PC_CR1=0b10000000;

    PE_DDR=0b10000000;
    PE_CR1=0b10000000;
    // буде мигати без конкретного часу 
    // таймерів ще не знаємо тому через 
    // делай
    long delay;
    for(;;){
        delay=DELAY;
      PC_ODR=0b10000000;
      PE_ODR=0b00000000;
      while(--delay){}
      delay=DELAY;
      PC_ODR=0b00000000;
      PE_ODR=0b10000000;  
      while(--delay){}
    };
}
```
