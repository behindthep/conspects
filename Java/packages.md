Если два класса с одинаковыми именами оказываются в одном проекте, программа не будет компилироваться.

Пакеты - механизм для организации классов в логически связанные группы. избежать конфликтов имен классов, разные пакеты могут иметь классы с одинаковыми именами.

Имя пакета соответствует директории проекта, в которых находятся файлы с исходным кодом.

имя пакета содержат префикс, закреплен за компанией или разработчиком. имя домена в обратном порядке. домен hexlet.io, пакеты начинаться с io.hexlet

пакеты организуются по функциональности или слоям приложения. создать пакет model, в котором основные сущности приложения — пользователи и курсы

```java
package io.hexlet.model; // Файл io/hexlet/model/User.java

public class User {
    public static String getGreeting(String userName) {
        return "Hello, " + userName + "!";
    }
}
```
Классы из одного пакета обращаться друг к другу по имени. Компилятор поймет, использовать класс User, расположенный в том же пакете
```java
package io.hexlet.model; // Файл io/hexlet/model/Course.java

public class Course {
    public static User getAuthor() {
    }
}
```

использовать классы из другого пакета, их нужно импортировать.

package io.hexlet;

import io.hexlet.model.User;

class App {
    public static void main(String[] args) {
        var greeting = User.getGreeting("John");
        System.out.println(greeting);
    }
}
Импортирование позволяет в коде обращаться к классу просто по его имени, иначе пришлось бы писать полное имя, включая название пакета (fully qualified):

var greeting = io.hexlet.model.User.getGreeting("John");
пакеты решают проблему конфликта имен, разные пакеты могут содержать классы с одинаковым именем. в одном месте нужны два класса из разных пакетов с одинаковым именем? полное имя класса:

package io.hexlet;

class App {
    public static void main(String[] args) {
        var name = io.hexlet.utils.User.getUserName();
        var greeting = io.hexlet.model.User.getGreeting(name);
        System.out.println(greeting);
    }
}

импортировать один из классов. Мы импортируем тот, который используем чаще всего. А для наименее используемого будем писать полное имя класса.

package io.hexlet;

import io.hexlet.model.User;

class App {
    public static void main(String[] args) {
        var name = io.hexlet.utils.User.getUserName();
        var greeting = User.getGreeting(name);
        System.out.println(greeting);
    }
}

импортировать все классы из пакета при помощи *

import java.util.*

Такой способ импортирует весь пакет целиком, в коде мы можем обратиться к любому классу пакета напрямую по имени. захламляет пространство имен, и классы из разных пакетов могут пересечься по именам.

Статический импорт
Java позволяет импортировать и использовать статические методы без явного указания класса. не нужно каждый раз указывать имя класса при вызове статического метода:

import static java.lang.Math.*;

public class Main {
    public static void main(String[] args) {
        // Можем опустить указание класса при вызове метода
        double result = sqrt(16);
        System.out.println(result);
    }
}
