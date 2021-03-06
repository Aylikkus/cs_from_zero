# Числа в памяти компьютера

## Зачем это нужно знать

Основная проблема которая может возникнуть при использовании компьютера - то, что он не представляет числа в привычном нам виде, заместо этого он использует двоичную (бинарную) систему счисления, в данной системе счисления число `2 = 10`, число `4 = 100` и т.д. по степеням двойки. Конечно в большинстве случаев мы этого не замечаем, так как все преобразования происходят вне нашего поля зрения, но иногда мы всё же можем заметить данные последствия, например при использовании чисел с плавающей запятой, так как при их использовании могут возникать ошибки округления (например `0.1 + 0.2 = 0.30000000000000004`)

## Целые числа

Для представления целых чисел существует два способа - знаковый и беззнаковый. Самый простой способ записать число в компьютер это просто записать его двоичное представление как оно есть, например число `13` мы можем записать просто как `1101`, таким образом можно сразу сказать, что под число отводится 4 бита (по числу разрядов в двоичном представлении) данный способ представления называется беззнаковым.

Если для представления целого числа необходим знак, то мы отводим под него только первый бит (считая слева). В данном случае число `13` приняло бы вид `01101`, а его отрицательный аналог `-13` `11101` (0 означает плюс, 1 означает минус), под такое число мы уже будем выделять не 4, а 5 бит.


## Дробные числа

Для представления дробных чисел используют способ представления с плавающей запятой. В данном способе первый бит(слева) отводится под знак, далее некоторое количество бит отводится под экспоненту, а оставшиеся биты отводятся под мантиссу. Что такое экспонента и мантисса? - для ответа на данный вопрос нам придётся обратиться к нормализованной экспоненциальной записи числа. В данной записи записывается целая часть числа, состоящая из одной цифры, разделённая запятой от оставшейся и умноженная на 10 в какой-либо степени. Например `12 = 1.2 * 10^1`, а `3.14 = 3.14 * 10^0` и т.д. В данном случае экспонентой является степень десятки(1 и 0 соответственно), а мантисса само дробное число (1.2 и 3.14 соответственно). Для представления дробных чисел избрали такой же метод, только в двоичном виде. 

Допустим у нас есть число с плавающей запятой, хранящей в себе 16 бит, под экспоненту у нас отводится 5 бит, а под мантиссу 10. Мы хотим записать число 14.2. Для начала преобразуем данное число в двоичное `14.2 = 1110.00110011001100110011`, в данном случае 0.2 (дробная часть числа) не может быть представлено в двоичном виде (тоже самое что и 1/3 в десятичной системе счисления, 0011 повторяется до бесконечности), поэтому нам придётся округлять число. Приводим число к нормализованной экспоненциальной записи `14.2 = 1.11000110011001100110011 * 2^3`, 2 в данном случае у нас является такое же десяткой, прошу не забывать. Знак положительный (0), экспонентой в данном случае у нас является 3, мантиссой 1.111000110011001100110011. 

Нужно учесть что экспонента также может быть как отрицательной так и положительной, для экспоненты знак вычисляется немного по другой логике, который называется смещение. Если у нас отводится 5 бит под экспоненту, то первый бит отводится под знак, а остальные 4 под само число. `1111 = 15` является смещением, к этому числу нужно добавить экспоненту (3) получаем 18. `18 = 10010` - итоговая экспонента, если бы например экспонентой являлось число `-3`, то после смещения мы бы получили число `12 = 01100`, так как мы храним 5 бит, то в начале добавляется лидирующий ноль. Это работает таким образом, что число после смещения потом просто вычитается из смещения, получаем экспоненту. Это сделано таким образом для удобства сравнения.

Для того чтобы записать мантиссу нам придётся отбросить лидирующую единицу (так как все двоичные числа в нормализованной форме начинаются с единицы) и записать то, что идёт после запятой. Учитываем размер в 10 бит `1100011001`. Итоговое число - `0 10010 1100011001`

## Проблема с дробными числами

Как можно было заметить данный способ записи чисел страдает от ошибок округления при переводе из двоичной в десятичную систему счисления и обратно. Поэтому не рекомендуется применять подобную запись при вычислениях, в которых важна точность, или например при совершении банковских операций. Также при сравнении может быть неожидаемый результат (как было показано в самом начале), всё это ошибки округления.

## Примеры в C#

C# предоставляет прямой способ записи чисел из двоичного формата, для этого нужно записать `0b` и само двоичное число.

```csharp
Console.WriteLine(0b101);
    // Вывод: 5
```

Также можно записывать числа в шестнадцатеричной системе счисления.

```csharp
Console.WriteLine(0xff);
    // Вывод: 255
```

Для хранения чисел C# предоставляет уйму различных типов. Самый распространённый это `int`, который хранит в себе 32 бита, где первый бит отводится под знак, можно посчитать максимальное значение:

```csharp
int n = 0b01111111111111111111111111111111;
Console.WriteLine(n);
    // Вывод: 2147483647
```

Также есть его аналог без знака `uint`, посчитаем максимальное значение:

```csharp
uint n = 0b11111111111111111111111111111111;
Console.WriteLine(n);
    // Вывод: 4294967295
```


Помимо этого есть типы для хранения 8 бит: `sbyte` (со знаком) и `byte` (без знака), 16 бит (`short` и `ushort`), 64 бит (`long` и `ulong`).

---

Для хранения чисел с дробной запятой нам предоставляют два типа: `float` (32 бита) и `double` (64 бита). По умолчанию каждому дробному числу в C# присваивается тип double, если мы явно не укажем другой тип, например:

```csharp
var d = 0.4;
Console.WriteLine(d.GetType());
    // Вывод: System.Double
```

Для того чтобы сделать это типом float нам нужно написать в конце постфикс `f`, например:

```csharp
var d = 0.4f;
Console.WriteLine(d.GetType());
    // Вывод: System.Single (аналог float, но в спецификации .net, по сути одно и тоже)
```

---

Для вычислений, в которых критически важна точность(например банковские операции) используется тип данных `decimal`, хранящий 128 бит. Для записи чисел в данном формате используется постфикс `m`. Из-за особенностей записи данного типа он медленнее чем  `float` или `double` (если программа делает большое кол-во операций с числами), но при этом он точно показывает число (например 0.1 это точно 0.1, а не округлённое число). Подробности хранения данного типа данных можно найти [тут](https://docs.microsoft.com/ru-ru/dotnet/api/system.decimal.-ctor?view=net-6.0#system-decimal-ctor(system-int32())), в разделе конструктора Decimal(Int32[]).

Рассмотрим пример где можно заметить отличия `decimal` от других подобных типов данных:

```csharp
var doubleNumber = 0.1 + 0.2;
var decimalNumber = 0.1m + 0.2m;

Console.WriteLine(doubleNumber == 0.3);   // Вывод: false
Console.WriteLine(decimalNumber == 0.3m); // Вывод: true
```

--- 

Зачем же мы пишем постфиксы `f` и `m`? Основная проблема в том, что дробные числа нельзя так же спокойно представлять между собой как это делают целые числа. У числа с плавающей запятой есть такое понятие как точность, т.е. сколько знаков может быть после запятой. Поэтому когда мы пишем дробные числа нам нужно точно указывать какую точность мы имеем ввиду. Например, мы не можем просто так взять и сравнить число типа `decimal` с числом типа `double`, так как это абсолютные разные вещи.

```csharp
Console.WriteLine(0.3 == 0.3m); // Ошибка
```