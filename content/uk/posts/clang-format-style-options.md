---
title: "Clang-Format"
date: 2020-12-08
lastmod: 2020-12-08
draft: false

# link: "http://protoelectriceffect.blogspot.com"

weight: 5
tags:
- C
- Cpp
- clang-format
- C#
- java
- javascript
- proto

description: "Параметри стилю Clang-Format"
---
<!-- https://clang.llvm.org/docs/ClangFormatStyleOptions.html# -->

## Параметри стилю clang-format

Параметри стилю `clang-format` описують налаштовувані параметри стилю форматування, що підтримуються `LibFormat` та `ClangFormat` .

Використовуючи утиліту командного рядка **clang-format** або функції `clang::format::reformat(...)` з коду, можна або використовувати один із заздалегідь визначених стилів ( `LLVM` , `Google` , `Chromium` , `Mozilla` , `WebKit` , `Microsoft` ) або створити власний стиль шляхом налаштування конкретних параметрів стилю.

### Налаштування стилю clang-format

**clang-format** підтримує два способи надання власних параметрів стилю: безпосередньо вкажіть конфігурацію стилю в опції командного рядка -style = або використовуйте файл -style = та додайте конфігурацію стилю у файл .clang-формату або _clang-формату у каталозі проекту.

Файл `.clang-format` використовує синтакисис `yaml` :

``` yaml {linenos=false}
key1: value1
key2: value2
# коментар
```

Приклад файла конфігурації для кількох мов

``` yaml {linenos=false}
---
# Ми використовуємо за замовчуванням LLVM стиль, але ...
BasedOnStyle: LLVM
# з відступом 4
IndentWidth: 4
---
# Для мови програмування C++
Language: Cpp
# Примусові вказівники на тип
DerivePointerAlignment: false
PointerAlignment: Left
---
# для JavaScript
Language: JavaScript
# обмежуємо довжину рядка до 100 символів
ColumnLimit: 100
---
# .proto файли
Language: Proto
# автоматично форматувати не будемо
DisableFormat: true
---
# Для C#
Language: CSharp
# використовуємо обмеження довжини рядка
ColumnLimit: 100
...
```

### Вимкнення форматування для фрагменту коду

Clang-format розуміє також спеціальні коментарі, які вимикають форматування на обраному форагменты коду. Код між коментарями `// clang-format off` або `/* clang-format off */` та `// clang-format on` та `/* clang-format on */` не буде форматуватися. Коментарі будуть відформатовані (вирівняні) у звичайному режимі.

``` cpp {linenos=false}
int formatted_code;
// clang-format off
    void    unformatted_code  ;
// clang-format on
void formatted_code_again;
```

### Конфігурація стилю у коді

<!-- When using clang::format::reformat(...) functions, the format is specified by supplying the clang::format:: FormatStyle structure. -->

### Настроювані параметри стилю формату

У цьому розділі перелічені підтримувані параметри стилю. Тип значення вказаний для кожного варіанту. Для типів перерахування можливі значення вказуються як як елемент переліку C++ (з префіксом, наприклад `LS_Auto` ), так і як значення, яке можна використовувати в конфігурації (без префікса: `Auto` ).

#### BasedOnStyle (__string__)

Стиль, який використовується для всіх параметрів, не вказаних у конфігурації.

Цей параметр підтримується лише у конфігурації формату clang (як у межах `-style = '{...}'` , так і у файлі `.clang-format` ).

Можливі значення:
   * `LLVM` cтиль, що відповідає [LLVM coding standards](https://llvm.org/docs/CodingStandards.html)
   * `Google` cтиль, що відповідає [Google’s C++ style guide](https://google.github.io/styleguide/cppguide.html)
   * `Chromium` cтиль, що відповідає [Chromium’s style guide](https://chromium.googlesource.com/chromium/src/+/master/styleguide/styleguide.md)
   * `Mozilla` cтиль, що відповідає [Mozilla’s style guide](https://developer.mozilla.org/en-US/docs/Developer_Guide/Coding_Style)
   * `WebKit` cтиль, що відповідає [WebKit’s style guide](https://www.webkit.org/coding/coding-style.html)
   * `Microsoft` cтиль, що відповідає [Microsoft’s style guide](https://docs.microsoft.com/en-us/visualstudio/ide/editorconfig-code-style-settings-reference?view=vs-2017)
   * `GNU` cтиль, що відповідає [GNU coding standards](https://www.gnu.org/prep/standards/standards.html)

``` yaml {linenos=false}
BasedOnStyle: Google
```

#### AccessModifierOffset (__int__)

Додатковий відступ або відступ модифікаторів доступу, напр. `public:` .

``` yaml {linenos=false}
AccessModifierOffset: 2
```

#### AlignAfterOpenBracket (__BracketAlignmentStyle__)

Якщо `true` , горизонтально вирівнює аргументи після відкритої дужки.

Це стосується круглих дужок `()` , кутових дужок `<>` та квадратних дужок `[]` .

Можливі значення:

  + `BAS_Align` (у конфігурації: `Align`) Вирівняти параметри на відкритій дужці, наприклад:

  

``` cpp {linenos=false}
  void someLongFunction(argument1,
                        argument2);
  ```

  + `BAS_DontAlign` (у конфігурації: `DontAlign`) Не вирівнювати, замість цього використовуйте `ContinuationIndentWidth`, наприклад:

  

``` cpp {linenos=false}
  void someLongFunction(argument1,
    argument2);
  ```

  + `BAS_AlwaysBreak` (у конфігурації: `AlwaysBreak`) Завжди розривати після відкритої дужки, якщо параметри не вміщуються в один рядок, наприклад:

  

``` cpp {linenos=false}
  void someLongFunction(
    argument1, argument2);
  ```

  #### AlignConsecutiveAssignments (__bool__)

  Якщо `true` , вирівнює послідовні призначення.

Це вирівняє оператори присвоєння послідовних рядків. Це призведе до форматування виду:

``` cpp {linenos=false}
int aaaa = 12;
int b    = 23;
int ccc  = 23;
```

#### AlignConsecutiveBitFields (__bool__)

Якщо `true` , вирівнює послідовних членів бітового поля.

Це вирівняє роздільники розрядних полів послідовних рядків. Це призведе до форматування виду:

``` cpp {linenos=false}
int aaaa : 1;
int b    : 12;
int ccc  : 8;
```

#### AlignConsecutiveDeclarations (__bool__)

Якщо `true` , вирівнює послідовні декларації.

Це вирівняє назви оголошень послідовних рядків. Це призведе до форматування виду:

``` cpp {linenos=false}
int         aaaa = 12;
float       b = 23;
std::string ccc = 23;
```

#### AlignConsecutiveMacros (__bool__)

Якщо значення `true` , вирівнює послідовні макроси препроцесора `C/C++` .

Це вирівняє макроси препроцесора `C/C++` послідовних рядків. Це призведе до форматування виду:

``` cpp {linenos=false}
#define SHORT_NAME       42
#define LONGER_NAME      0x007f
#define EVEN_LONGER_NAME (2)
#define foo(x)           (x * x)
#define bar(y, z)        (y + z)
```

#### AlignEscapedNewlines (__EscapedNewlineAlignmentStyle__)

Варіанти вирівнювання зворотних скісних рисок у рядах, що відключаються.

Можливі значення:

  + `ENAS_DontAlign` (у конфігурації: `DontAlign`) Не вирівнювати екрановані нові рядки.

  

``` cpp {linenos=false}
  #define A \
  int aaaa; \
  int b; \
  int dddddddddd;
  ```

  + `ENAS_Left` (у конфігурації: `Left`) Вирівняти екрановані нові рядки якомога далі ліворуч.

  

``` cpp {linenos=false}
  true:
  #define A   \
  int aaaa;   \
  int b;      \
  int dddddddddd;

  false:
  ```

  + `ENAS_Right` (у конфігурації: `Right`) Вирівнювання екранованих рядків у крайньому правому стовпці.

  

``` cpp {linenos=false}
  #define A                                                                      \
  int aaaa;                                                                      \
  int b;                                                                         \
  int dddddddddd;
  ```

#### AlignOperands (__OperandAlignmentStyle__)

Якщо `true`, горизонтально вирівняйте операнди бінарних та тернарних виразів.

Можливі значення:
+ `OAS_DontAlign` (у конфігурації: `DontAlign`) Не вирівнювати операнди бінарних та тернарних виразів. Обернуті рядки мають відступи `ContinuationIndentWidth` пробіли від початку рядка.

+ `OAS_Align` (у конфігурації: `Align`) Горизонтально вирівнює операнди бінарних та тернарних виразів.

Зокрема, це вирівнює операнди одного виразу, який потрібно розділити на кілька рядків, наприклад:

```cpp {linenos=false}
int aaa = bbbbbbbbbbbbbbb +
          ccccccccccccccc;
```
Коли встановлено `BreakBeforeBinaryOperators`, обернутий оператор вирівнюється з операндом у першому рядку.
```cpp {linenos=false}
int aaa = bbbbbbbbbbbbbbb
          + ccccccccccccccc;
```
+ `OAS_AlignAfterOperator` (у конфігурації: `AlignAfterOperator`) Горизонтально вирівнюйте операнди бінарних та тернарних виразів.

Це схоже на `AO_Align`, крім випадків, коли встановлено `BreakBeforeBinaryOperators`, оператор не має відступу, щоб загорнутий операнд вирівнювався з операндом у першому рядку.
```cpp {linenos=false}
int aaa = bbbbbbbbbbbbbbb
        + ccccccccccccccc;
```

#### AlignTrailingComments (__bool__)

Якщо `true`, вирівнює кінцеві коментарі.

```cpp {linenos=false}
true:                                   
int a;     // My comment a   
int b = 2; // comment  b                

// vs

false:
int a; // My comment a
int b = 2; // comment about b
```

#### AllowAllArgumentsOnNextLine (__bool__)

Якщо виклик функції або списковий ініціалізатор не вміщується у рядок, дозволяє розміщувати всі аргументи в наступному рядку, навіть якщо `BinPackArguments` хибний.
```cpp {linenos=false}
true:
callFunction(
    a, b, c, d);

false:
callFunction(a,
             b,
             c,
             d);
```

#### AllowAllConstructorInitializersOnNextLine (__bool__)

Якщо визначення конструктора зі списком ініціалізатора-члена не вміщується в одному рядку, дозволяє розмістити всі члени-ініціалізатори в наступному рядку, якщо `ConstructorInitializerAllOnOneLineOrOnePerLine` має значення `true`. Зауважте, що цей параметр не впливає, якщо значення `ConstructorInitializerAllOnOneLineOrOnePerLine` встановлено `false`.

```cpp {linenos=false}
true:
MyClass::MyClass() :
    member0(0), member1(2) {}

false:
MyClass::MyClass() :
    member0(0),
    member1(2) {}
```

#### AllowAllParametersOfDeclarationOnNextLine (__bool__)

Якщо оголошення функції не вміщується в рядку, дозволяє помістити всі параметри оголошення функції в наступний рядок, навіть якщо `BinPackParameters` встановлено `false`.

```cpp {linenos=false}
true:
void myFunction(
    int a, int b, int c, int d, int e);

false:
void myFunction(int a,
                int b,
                int c,
                int d,
                int e);
```

#### AllowShortBlocksOnASingleLine (__ShortBlockStyle__)

Залежно від значення, `while (true) { continue; }` можна поставити в один рядок.

Можливі значення:
+ `SBS_Never` (у конфігурації: `Never`) Ніколи не об’єднуйте блоки в один рядок.
```cpp {linenos=false}
while (true) {
}
while (true) {
  continue;
}
```
+ `SBS_Empty` (у конфігурації: `Empty`) Об’єднувати лише порожні блоки.
```cpp {linenos=false}
while (true) {}
while (true) {
  continue;
}
```
+ `SBS_Always` (у конфігурації: `Always`) Завжди об’єднуйте короткі блоки в один рядок.
```cpp {linenos=false}
while (true) {}
while (true) { continue; }
```

#### AllowShortCaseLabelsOnASingleLine (__bool__)

Якщо `true`, короткі мітки будуть стискатися в один рядок.
```cpp {linenos=false}
true:                                   
switch (a) {                        
case 1: x = 1; break;                   
case 2: return;                         
}     

// vs.                                                                              

false:
switch (a) {
case 1:
  x = 1;
  break;
case 2:
  return;
}
```

#### AllowShortEnumsOnASingleLine (__bool__)

Дозволити короткі переліки в одному рядку.

```cpp {linenos=false}
true:
enum { A, B } myEnum;

false:
enum
{
  A,
  B
} myEnum;
```

#### AllowShortFunctionsOnASingleLine (__ShortFunctionStyle__)

Залежно від значення, `int f () {return 0; }` можна поставити в один рядок.

Можливі значення:

+ `SFS_None` (у конфігурації: `None`) Ніколи не об’єднуйте функції в один рядок.

+ `SFS_InlineOnly` (у конфігурації: `InlineOnly`) Лише функції злиття, визначені всередині класу. Те саме, що і "вбудований", за винятком того, що це не означає "порожній": тобто порожні функції верхнього рівня також не об'єднуються.
```cpp {linenos=false}
class Foo {
  void f() { foo(); }
};
void f() {
  foo();
}
void f() {
}
```
+ `SFS_Empty` (у конфігурації: `Empty`) Об'єднати лише порожні функції.
```cpp {linenos=false}
void f() {}
void f2() {
  bar2();
}
```
+ `SFS_Inline` (у конфігурації: `Inline`) Лише функції злиття, визначені всередині класу. Мається на увазі "порожній".
```cpp {linenos=false}
class Foo {
  void f() { foo(); }
};
void f() {
  foo();
}
void f() {}
```
+ `SFS_All` (у конфігурації: `All`) Об’єднати всі функції, що розміщуються в одному рядку.
```cpp {linenos=false}
class Foo {
  void f() { foo(); }
};
void f() { bar(); }
```
#### AllowShortIfStatementsOnASingleLine (__ShortIfStyle__)

Якщо `true`, `if (a) return;` можна поставити в один рядок.

Можливі значення:

+ `SIS_Never` (у конфігурації: `Never`) Ніколи не ставте короткі if'и на одному рядку.
```cpp {linenos=false}
if (a)
  return ;
else {
  return;
}
```
+ `SIS_WithoutElse` (у конфігурації: `WithoutElse`) Без інакшого ставити короткі if'и в тому ж рядку, лише якщо `else` не є складеним оператором.
```cpp {linenos=false}
if (a) return;
else
  return;
```
+ `SIS_Always` (у конфігурації: `Always`) Завжди ставіть короткі if'и в одному рядку, якщо `else` не є складеним твердженням чи ні.
```cpp {linenos=false}
if (a) return;
else {
  return;
}
```

#### AllowShortLambdasOnASingleLine (ShortLambdaStyle)

Залежно від значення, `auto lambda [] () {return 0; }` можна поставити в один рядок.

Можливі значення:


#### =======================

``` cpp {linenos=false}
int aaaa : 1;
int b    : 12;
int ccc  : 8;
```

### Приклади

Стиль який симулює [Linux Kernel style](https://www.kernel.org/doc/Documentation/CodingStyle)

``` yml {linenos=false}
BasedOnStyle: LLVM
IndentWidth: 8
UseTab: Always
BreakBeforeBraces: Linux
AllowShortIfStatementsOnASingleLine: false
IndentCaseLabels: false
```

Результат (уявіть, що тут для відступу використовуються табуляція):

``` cpp {linenos=false}
void test()
{
        switch (x) {
        case 0:
        case 1:
                do_something();
                break;
        case 2:
                do_something_else();
                break;
        default:
                break;
        }
        if (condition)
                do_something_completely_different();

        if (x == y) {
                q();
        } else if (x > y) {
                w();
        } else {
                r();
        }
}
```

Стиль, подібний до стилю форматування `Visual Studio` за замовчуванням:

``` yml {linenos=false}
UseTab: Never
IndentWidth: 4
BreakBeforeBraces: Allman
AllowShortIfStatementsOnASingleLine: false
IndentCaseLabels: false
ColumnLimit: 0
```

Результат:

``` cpp {linenos=false}
void test()
{
    switch (suffix)
    {
    case 0:
    case 1:
        do_something();
        break;
    case 2:
        do_something_else();
        break;
    default:
        break;
    }
    if (condition)
        do_somthing_completely_different();

    if (x == y)
    {
        q();
    }
    else if (x > y)
    {
        w();
    }
    else
    {
        r();
    }
}
```

[оригінал тут](https://clang.llvm.org/docs/ClangFormatStyleOptions.html#)
