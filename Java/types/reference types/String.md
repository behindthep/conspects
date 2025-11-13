String - массив символов.

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


Под хранение значения переменной выделяется область памяти (Память — область для хранения данных, склад). Значение получает номер (адрес), по которому его можно извлечь и заменить.

- Примитивные данные сравниваются по значению, независимо от адресов
- Ссылочные данные сравниваются по адресам

Если бы строка всегда вела себя как ссылочный тип, то на каждое значение в коде выделялась доп. память.
Без оптимизаций это выражение привело бы к двойному выделению памяти. По одной единице памяти на каждый "hm"
```java
"hm" == "hm"; // true
"hexlet".toUpperCase() == "hexlet".toUpperCase(); // false
```

Когда Java встречает явно создаваемую строку, проверка на сущ уже в памяти.
Если есть, то она переиспользуется, если нет — создается:
```java
// Выделяется память
var name1 = "Java"; // строка уже есть, подставляется ссылка на уже созданную строку - экономится память
var name2 = "Java";
// Сравнение по ссылке Обе переменные указывают на один участок памяти
name1 == name2; // true
```

если строка возвращается из метода, то она помещается в свою область памяти со своим уникальным адресом:
```java
// Выделяется новая память в любом случае
var name1 = "java".toUpperCase(); // "JAVA"
// Выделяется новая память в любом случае
var name2 = "java".toUpperCase(); // "JAVA"
name1 == name2; // false
```

сравниваем строки по значению, чем по ссылке - методы.
```java
name1.equals(name2); // true
name1.equalsIgnoreCase(name3); // true
```
