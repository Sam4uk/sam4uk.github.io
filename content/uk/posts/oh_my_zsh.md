---
title: Підкорюємо zsh
date: 2020-12-20
description: Тюнінг термніналу Linux
draft: false
hideToc: false
enableToc: true
enableTocContent: true
# author: Samchuk
authorImageUrl: https://avatars.githubusercontent.com/u/15850554?v=4
# authorEmoji: 🤖
tags:
- linux
- bash
- zsh
- terminal
categories:
- tutorials
series:
- Linux
# image: images/feature1/markdown.png
---
<!-- ---
title: "Підкорюємо zsh"
description : "Тюнінг термніналу Linux"
# weight: 1
date: 2020-12-20
tags:
- linux
- bash
- zsh
- terminal
categories:
- tutorials
cover: "https://trofimovdigital.ru/wp-content/uploads/oh-my-zsh/oh-my-zsh.jpg"
draft: fasle
# license: "BY-NC-SA"
--- -->

## Вступ

Коли я працював з Win системами я не любив командного 
інтерфейсу, що пропонувався у цій системі. Командний 
рядок був монохромним та гнітючим - чорний та білий.
Так... Командний рядок можна тюнінгувати, але в дуже
обмеженому діапазоні: змінити колір фону та шрифту,
змінити розмір літер та їх гарнітуру і запрошення тай 
все.
Я завжди боявся запускати на виконаня команди з cmd. 
Та взагалі вінда спонукає використовувати графічний 
інтерфейс. Майже все у вінді можна робити без 
використання клавіатури.

Інша справа у `Linux`. Після мого знайомства з цією 
системою страх перед `CLI` почав розвіюватися. Навіть
не знаю чере що. Чи через те що термінал більш потужний
інструмент, чи через те що він більш гнучкий, чи через
те що командний рядок є невід'єсною частиною системи.

Все що можна виконти графічним маніпулятором (мишою)
те можна продублювати у терміналі і навпаки. `Windows`
не може похизуватися такими можливостями.

Сьогодні будемо тюнінгувати термінал. 

Останнім часом в мене почав зникати страх та з'являтися
любов до `CLI`. Це мабуть завдяки `zsh` :)

## Встановлення zch

В моїй системі за замовчуванням встановлена оболонка 
терміналу `bash`. Ми зараз це виправимо. Відкриємо
термінал

```bash 
sudo apt-get install zsh
```

Цією командою ми зупустили процес встановлення `Z-shel`.

```bash
zsh
```
Запускаємо `zsh`. При першому запуску оболонка запропонує
вам кілька питань для попереднього налаштування. Можна дати
відповіді на питання, або продовжити користуватись оболонкою
налаштуваннями за замовчуванням.

## Змінюємо оболонку за замовчуванням.

За засовчуванням в `Linux Mint` встановлена оболонка `bash`.
Виможете перевірити, який оболонка встановлена за замовчуванням
виконавши наступну команду

```bash
echo $SHELL
```
У відповіді ви отримаєте шлях до оболонки  терміналу, який
встановлено за замовчуванням.

Щоб при настисканні `Ctrl`+`Alt`+`T` старував `Z-shel` потрібно 
виконати команду: 
```bash
chsh -s /bin/zsh
```

## Zsh: швидкий старт

### Повторити команду
Ще до встановлення `Oh My Zsh` та плагінів можна оцінити 
можливості `zsh`

Спробуйте виконати наступну команду:

```zsh
!!
```
Вона вставить в консоль попередню команду. Буде корисна, якщо забули 
вказати `sudo` для команди яка вимагає підвищених прівілей.

Наприклад:
```zsh
you-sudo-command
```
Затребує права `root`:
```
error: you cannot perform this operation unless you are root.
```
У відповідь на це повідомлення можна виконати команду:
```zsh
sudo !!
```
`zsh` замінить її на:
```zsh
sudo you-sudo-command
```

### Повторити аргумент

Від попередньої команди можна отримати тільки аргумент

```zsh
cd ~/MyFolder
```
Якщо каталогу `~/MyFolder` не існує консоль виведе повідомлення, що не можливо
перейти до такої директорії
```
cd: no such file or directory: cd ~/MyFolder
```
Тоді ми можемо її створити командою
```zsh
mkdir !*
```
Оболонка зробить заміну `!*` на аргумент попередньої команди:
```zsh
mkdir ~/MyFolder
```

### Повторити команду за фрагментом

Вставити в консоль останню команду, яка починається з вказаних символів.
Для цього потрібно перед початком введень команди поставити знак оклику `!`:

```zsh
!part-of-command
```
Вставити в консоль попередню команду в якої відома лише частина символів по середині або в кінці:
```zsh
!?part-of-command?
```
<!-- Наприклад `!?VIDEO?` з моїєї історії команд після натиснення `Tab`, перетвориться в:
```zsh
git push --set-upstream origin dt.feature.VIDEO-10000
```

Опечатку в последней введенной команде можно исправить так:

^dc^cd

А с помощью следующей команды удобно сделать бэкап файла.

cp nginx.conf{,.bak}

Конструкция выше аналогична команде:

cp nginx.conf nginx.conf.bak

Следующая конструкция удалит ранее распакованные файлы. Будет полезна если вы распаковали архив tar.gz не в тот каталог.

rm -f `tar ztf /path/to/file.tar.gz`

Примеры выше, малая доля того, что умеет Zsh. Еще больше возможностей открывается при использовании фреймворка Oh My Zsh. -->

## Oh, my zsh

[Oh My Zsh](https://ohmyz.sh/) — open source фреймворк, що підтримується
спільнотою. Призначений для керування налаштуванням `zsh` та розширює його 
функціонал за рахунок тем та плагінів.

Лінк на [репозиторiй](https://github.com/ohmyzsh/ohmyzsh).
![](https://trofimovdigital.ru/wp-content/uploads/oh-my-term/oh-my-zsh-768x474.png)

#### Встановлення через curl

```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### Встановлення через wget

```zsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
#### Встановлення через вручну

 (загрузите скрипт, затем выполните его)
```zsh
curl -Lo install.sh https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh

sh install.sh
```

`Oh My Zsh` встановлюється в папку `~/.oh-my-zsh`. Якщо знадобиться видалити `Oh My Zsh`, не вмдаляйте теку вручну, а скористайтеся спеціальною командою:
```zsh
uninstall_oh_my_zsh
```