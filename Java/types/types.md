- Syntax errors are detected **during compilation**, before the program starts running.


- At the basic level, computers operate only with **numbers**.


- In Java, **integer division** is used **by default**: 3 / 2 = 1


**null** - is not type, it's default value for **reference types** when val is not defined.

```java
String a; // null
```

null and primitive types are not compatible. Primitive val always should be declared.


### type conversion

```java
// obvious
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
