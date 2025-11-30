All not primitive (reference) types are **objects**.

- Java is **statically typed** language - the var type is set when it declared and cannot be changed afterwards.

assign a number to the String var, you will get an error: incompatible types: int cannot be converted to java.lang.String.

The **compiler** performs this check without running the code - this type of typing called static typing.
In dynamic languages, the same behavior would not cause an error - a var can change its type during execution.


- Syntax errors are detected **during compilation**, before the program starts running.

- **integer division** is used **by default**: 3 / 2 = 1

**null** - is not type, it's default value only for **reference types** when значение is not defined.

```java
String a; // null
```

null and primitive types are not compatible. Primitive val always should be declared.


### type conversion

```java
var n = Integer.parseInt("345"); // 345
var res = (int) 5.1; // 5 for conversion between primitive types
```

Implicit type conversion
Some conversion can be automatically happen if in one expression participate different types - for it need that type be compatible beside each other and dimension conversioned types was equal or lower than result type.

```java
var a true;
// Error: incompatible types: int cannot be converted to boolean
a = 1;

short a = 5; // 2 bytes in memory
int sum = a + 1; // 4 bytes now

// Loss of accuracy:
int a = 5;
// Error: incompatible types: possible lossy conversion from int to short
short sum = a + 2;

var a = 10;
var res = "Number: " + a; // Number: 10 - 10 to String representation
```



**Сильная — слабая**
системы типов делятся по способу приведения типов - превращение данных одного типа в данные другого типа. «12» превратить в 12:
number age = toNumber("12")

Некоторые языки сами приводят типы, скрыто от программиста - слабой (или нестрогой) типизацией. в одном выражении использовать переменные любых типов и не беспокоиться об их приведении.
print("12" + 13) // 1213
print(12 + "13") // 25

языки, которые требуют явно определить, что следует делать с данными, чтобы перевести их в другой тип. отдают эту работу программисту - сильной (или строгой) типизацией.
print("12" + 13) // Ошибка!
print("12" + toString(13)) // 1213
print(toNumber("12") + 13) // 25

**Статическая — динамическая**
В статически типизированном языке каждая переменная имеет определенный тип на всем протяжении жизни, он не может измениться во время выполнения. все типы известны ещё на этапе написания кода. Если же во время выполнения попытаться присвоить переменной одного типа значение другого типа, произойдет ошибка. такие ошибки можно найти без запуска программы.
number age = 44
age = "Not so old" // Ошибка!

Динамически типизированные языки у каждой переменной всё ещё есть тип. Но он может меняться по ходу исполнения программы. в конкретный момент времени мы достоверно не знаем, данные какого типа находятся в переменной.
age = 44 // Тип переменной — число
age = "Not so old" // Тип переменной — строка

Java — сильная статическая

1 байт = 8 бит

byte 1 байт	-128 до 127
short 2 байта	-32 768 до 32 767
int* 4 байта	-2 147 483 648 до 2 147 483 647
long 8 байт	-9 223 372 036 854 775 808 до 9 223 372 036 854 775 807 ≈ −9.22 × 1018 ... ≈ 9.22 × 1018

float (floating-point number — число с плавающей точкой) 4 байта	3.14f	низкая, одинарная точность (7 знаков после запятой)
double* 8 байт	-1.7E+308	высокая, двойная точность (16 знаков после запятой)

boolean 1 бит

явно указать, какой тип использовать для числа. не указать суффикс, целое число int, а дробное — double.
L или l — типа long (10000000000L) // если убрать L, ошибка компиляции
F или f — типа float (3.14f)
D или d — типа double (не нужен, дробные числа без суффикса по дефолту — double)


Тип char для хранения одного символа: буквы, цифры, знака препинания, пробела, спецсимвола и смайлика. Значение типа char в одиночных кавычках.
За каждым символом скрывается числовой код (стандарт Unicode - символы со всего мира). хранить латинские буквы, кириллицу, иероглифы, эмодзи и древнеегипетские иероглифы.
Тип char — 16-битное число (от 0 до 65535), каждому значению соответствует свой символ.

каждый символ внутри — число, преобразовывать char в int и обратно. узнать Unicode-код символа

```java
char ch = 'A';
int code = ch; // Неявное преобразование char → int
System.out.println("Код символа '" + ch + "': " + code); // Код символа 'A': 65

int code = 1040; // Код буквы 'А' в Юникоде (кириллица)
char ch = (char) code; // Явное преобразование int → char
System.out.println("Символ с кодом " + code + ": " + ch); // Символ с кодом 1040: А
```


double d = 3; не получите ошибку — типы автоматически приведутся (целое число превращается во "вещественное" без потерь).

Scanner.nextDouble() ориентируется на локаль: в русской локали запятая допустима, в английской — точка. настройте локаль для Scanner или читайте строку и парсьте вручную.

System.out.println(0.1 + 0.2); //0.30000000000000004 // компьютер многие числа не может точно представить в двоичной системе.


int ii = (int) 3.7; // 3 - явно привести double к int

System.out.println(String.format("%.2f", 23.56789)); // 23.57
System.out.println(String.format("%.1f%n", 23.56789)); // 23.6

неявное преобразование double в float
float f = 1.23; // Ошибка! положить double в float — может привести к потере точности! добавляй суффикс f.


double result = 7 / 2; // деление двух int — int-результат 3.0
double result = (double) 7 / 2; // 3.5

Не сравнивай дробные с ==, сравнивай с небольшим допуском (epsilon).


