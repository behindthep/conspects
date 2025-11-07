String представляет собой массив символов.


Strings are **immutable**. String method can only return new string, not change existent.
Create new based on old val. Strings Immutability worth for memory optimization, string can be cached in String Pool, also for safety in multithreaded environment.
When use string concatenation many temporary object created, so it can decrease performance.

```java
var greet = "Hello"; // string store in specific memory space called 'String Pool'
```

String literals - strings that placed inside "literals". They automatically saved in String Pool, it let Java efficiently manage memory and decrease quantity of duplicating strings.

When string create with literal, JVM checks, if exists the same string in Pool. If she finded, then returns link to it, if doesn't - create new string and place it in Pool.

```java
var greet =  new String("Hello";) // This way created new string object, even if same string already exist in Pool. Because of it using literals are preferable.

var sub = greet.substring(0, 3).toUpperCase(); // "HEL". 'greet' is a object of class String and substring is a method of class String.
```


Под хранение переменной выделяется область памяти. Программа запоминает адрес этой области и работает с ней - считала значение переменной по адресу, где хранится значение.

Память — область для хранения данных, похожа на склад. В памяти значение получает номер (адрес), по которому его можно извлечь и заменить.


Примитивные данные сравниваются по значению, независимо от адресов
Ссылочные данные сравниваются по адресам

int[] a = {1, 2}
int[] b = {1, 2}
// Значения одинаковые, но ссылки разные
a == b; // false


"hm" == "hm"; // true
"hexlet".toUpperCase() == "hexlet".toUpperCase(); // false

Если бы строка всегда вела себя как ссылочный тип, то на каждое значение в коде выделялась доп память:

// Без оптимизаций это выражение привело бы к двойному выделению памяти. По одной единице памяти на каждый "hm"
"hm" == "hm";
Когда Java встречает явно создаваемую строку, происходит проверка на сущ уже в памяти такой строки.
Если есть, то она переиспользуется, если нет — создается:

// Выделяется память
var name1 = "Java"; // Такая строка уже есть, подставляется ссылка на уже созданную строку - экономится память
var name2 = "Java";
// Сравнение по ссылке
// Обе переменные указывают на один участок памяти
name1 == name2; // true
если строка возвращается из метода, то она помещается в свою область памяти со своим уникальным адресом:

// Выделяется новая память в любом случае
var name1 = "java".toUpperCase(); // "JAVA"
// Выделяется новая память в любом случае
var name2 = "java".toUpperCase(); // "JAVA"
name1 == name2; // false

сравниваем строки по значению, чем по ссылке используйте методы.
var name1 = "java".toUpperCase(); // "JAVA"
var name2 = "java".toUpperCase(); // "JAVA"
name1.equals(name2); // true

var name3 = "java".toLowerCase(); // "java"
name1.equalsIgnoreCase(name3); // true
