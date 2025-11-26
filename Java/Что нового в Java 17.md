Switch-выражения из Java 14
получить значение из switch-выражения, раньше приходилось создавать отдельную переменную и постоянно использовать break;. Вот как это выглядело:

String season;
switch (month) {
    case JANUARY:
    case FEBRUARY:
        season = "winter";
        break;
    case MARCH:
    case APRIL:
    case MAY:
        season = "spring";
        break;
    case JUNE:
    case JULY:
    case AUGUST:
        season = "summer";
        break;
    case SEPTEMBER:
    case OCTOBER:
    case NOVEMBER:
        season = "autumn";
        break;
    case DECEMBER:
        season = "winter";
        break;
    default:
        throw new IllegalArgumentException();
    	}

В Java 14 появился новый формат записи, который помогает получать результат выбора и записывать выражение компактнее. Если перечислены все возможные варианты, ветка default теперь не нужна:

String season = switch (month) {
    case JANUARY, FEBRUARY -> "winter"; //несколько вариантов
    case MARCH, APRIL, MAY -> "spring";
    case JUNE, JULY, AUGUST -> "summer";
    case SEPTEMBER, OCTOBER, NOVEMBER -> "autumn";
    case DECEMBER -> "winter"; //oдин вариант
}

Но веткой default можно задать сообщение об ошибке:

String season = switch (month) {
    case "JAN", "FEB" -> "winter";
    default -> throw new IllegalArgumentException("no such case:" + month);
}

Если в значение (case) нужно записать выражение, его заключают в фигурные скобки {} и для возврата значения используют ключевое слово yield:

String season = switch (month) {
    case JANUARY, FEBRUARY -> "winter";
    case MARCH, APRIL, MAY -> "spring";
    case JUNE, JULY, AUGUST -> "summer";
    case SEPTEMBER, OCTOBER, NOVEMBER -> {
        System.out.println("winter is coming!");
        yield "autumn";
    }
    case DECEMBER -> "winter";
}
Switch-выражениям не обязательно возвращать определённое значение:

switch (order) {
    case NATURAL -> Arrays.sort(strings, Comparator.naturalOrder());
    case REVERSE -> Arrays.sort(strings, Comparator.reverseOrder());
}

Текстовые блоки из Java 15
Если раньше нужно было использовать литерал в несколько строк, его собирали через конкатенацию:

String query = "SELECT Students.name as \"Name\", SUM(Courses.duration) as \"Duration\"\n"
   + "FROM Students\n"
   + "JOIN Subscriptions ON Students.id = Subscriptions.student_id\n"
   + "JOIN Courses ON Subscriptions.course_id = Courses.id\n"
   + "GROUP BY Students.id\n"
   + "ORDER BY Students.name";

как появились текстовые блоки, это делают одним блоком. Текст с переносами и кавычками заключают в тройные кавычки:

String query = """
   SELECT Students.name as "Name", SUM(Courses.duration) as "Duration"
   FROM Students
   JOIN Subscriptions ON Students.id = Subscriptions.student_id
   JOIN Courses ON Subscriptions.course_id = Courses.id
   GROUP BY Students.id
   ORDER BY Students.name""";
System.out.println(query);

код отобразится при выводе в консоль (примечание: слева не будет пробелов или отступов):

Размер отступа зависит от отступа в предыдущей строке:

String query = """
   SELECT Students.name as "Name", SUM(Courses.duration) as "Duration"
       FROM Students
       JOIN Subscriptions ON Students.id = Subscriptions.student_id
       JOIN Courses ON Subscriptions.course_id = Courses.id
       GROUP BY Students.id
       ORDER BY Students.name""";
System.out.println(query);


Левая граница строки общая для всего блока, правую определяет последний символ каждой строки:

Если знак """ поставить не за последним элементом, а разместить на новой строке, то после каждой строки появится перенос:

Java 15 и выше:

String query = """
   line1
   line2
   line3
   """;
Ранние версии Java:

String query = "line1\nline2\nline3\n";
разместить всё в одну строку, нужно экранировать переносы — добавлять в конце строк символ \:

Java 15 и выше:

String query = """
   line1\
   line2\
   line3""";
Ранние версии Java:

String query = "line1line2line3";
Писать многострочные тексты стало намного проще — код приятно читается.

Новые варианты применения instanceof из Java 16
Чтобы проверить, к какому классу относится объект, используют оператор instanceof. Если нужно проверить объект и привести его к нужному виду, раньше объявляли переменную, присваивали ей тип, а затем проверяли объект:

Object string = "this is string!";

if(string instanceof String){
   String realString = (String) string;
   System.out.println(realString);
}

с Java 16 присвоение не требуется. Значение переменной можно задать прямо в выражении:

if(string instanceof String realString){
   System.out.println(realString);
}

Если условие проверки не выполняется, оператор instanceof не ограничивается фигурными скобками внутри условия if, а проверяет код дальше:

Object object = 23;

if (!(object instanceof Number number)) {
   throw new IllegalArgumentException("this is not a Number!");
}

System.out.prin
проверка !(object instanceof Number number) выдаёт результат false, и после выхода из if мы можем использовать number для реализации своей логики.


Класс record из Java 16
Класс record — писать иммутабельные POJO-классы, с ней не нужно повторять одинаковые методы: геттеры, toString(), equals() и hashCode().

старым методом класс Student c двумя полями — именем и названием факультета:

public class Student {
   private final String name;
   private final CourseType courseType;

   public Student(String name, CourseType courseType) {
       this.name = name;
       this.courseType = courseType;
   }

   public String getName() {
       return name;
   }

   public CourseType getCourseType() {
       return courseType;
   }

   @Override
   public boolean equals(Object o) {
       if (this == o) {
           return true;
       }
       if (o == null || getClass() != o.getClass()) {
           return false;
       }
       Student student = (Student) o;
       return name.equals(student.name) && courseType == student.courseType;
   }

   @Override
   public int hashCode() {
       return Objects.hash(name, courseType);
   }

   @Override
   public String toString() {
       return "Student{" +
              "name='" + name + '\'' +
              ", courseType=" + courseType +
              '}';
   }
}

У класса всего два поля, но много бойлерплейтного (Бойлерплейт — код, который вставляют в приложение почти без изменений. Иногда одни и те же шаблоны кода копируют в разные части приложения.) кода.

как работает класс, сделаем два объекта, выведем их в консоль и сравним:

var student = new Student("Alex", CourseType.MATH);
var studentSame = new Student("Alex", CourseType.MATH);

System.out.println(student);
System.out.println(studentSame.equals(student));
System.out.println(studentSame.hashCode() == student.hashCode());

До того как в Java появился класс record, нам помогал плагин и библиотека Lombok — она позволяет указать аннотациями, какие методы сгенерировать для класса на этапе компиляции. С ней класс может выглядеть так:

@Data
@RequiredArgsConstructor
public class Student {
    private final String name;
    private final CourseType courseType;
}

@Data генерирует геттеры, сеттеры, equals(), hashCode() и toString(). @RequiredArgsConstructor создаёт конструктор с итоговыми параметрами класса и добавляет аргументы в порядке объявления. Такая запись намного компактнее, но это даётся ценой лишней зависимости и плагина для среды разработки.

Если запустить тестовый код, видно, что формат преобразования в строку отличается, но в остальном всё работает одинаково:

Класс record избавляет от бойлерплейтного кода. Для этого не нужны внешние плагины, достаточно встроенных возможностей языка. создать класс, указать только два поля — конструктор, геттеры, equals(), hashCode() и toString() уже включены в класс.

public record Student(String name, CourseType courseType) {}
Запустим тестовый код и увидим тот же результат:

У геттеров больше нет приставки get., к методу мы обращаемся по имени переменной:

student.name(); //Alex
student.courseType(); //MATH
Если нужна валидация данных, конструктор можно расширить или написать свой:

Расширенный конструктор:

public record Student(String name, CourseType courseType) {

   public Student {
       if (name == null || name.isBlank()) {
           throw new IllegalArgumentException();
       }
   }
}

Кастомный конструктор:

public record Student(String name, CourseType courseType) {

   public Student(String name){
       this(name, CourseType.MATH);
   }
}

стандартный конструктор со всеми параметрами класса остаётся доступным.

У класса record есть ограничения и особенности:

все объявленные поля получают модификатор final (Если класс объявили с модификатором final, от него нельзя наследоваться.);
все поля класса объявляются в заголовке, дополнительные объявить нельзя:

//ошибка компиляции:
public record Student(String name, CourseType courseType) {
	private int id;
}

можно объявлять static-поля класса;
класс record неявно объявлен как final, поэтому его нельзя наследовать;
он не может быть абстрактным и наследовать другие классы;
можно добавлять свои конструкторы;
для конструктора можно использовать проверку аргументов;
можно переопределить стандартные методы — геттеры, toString(), equals() и hashCode();
можно добавлять статические и нестатические методы.
Благодаря компактному синтаксису класс records несложно обновлять локально:

public static List<Student> findTreeStudentsByAlphabet(List<Student> students) {
   record CourseTypeToFirstLetter(Student student, char firstLetter) {}

   return students.stream()
       .map(st -> new CourseTypeToFirstLetter(st, st.name().charAt(0)))
       .sorted((Comparator.comparing(CourseTypeToFirstLetter::firstLetter)))
       .map(CourseTypeToFirstLetter::student)
       .limit(3)
       .collect(Collectors.toList());
}

структура данных хранится в этом же методе, нет необходимости создавать её в другом месте и слишком расширять область видимости.

Запечатанные классы из Java 17
Sealed class дословно переводится как «запечатанный класс». В этом классе нужно сразу объявить список классов-наследников, потому что кроме них наследников быть не может. похоже на enum, только в разрезе наследования.

sealed class Person permits Student, Teacher, Curator {}
Классы Student, Teacher и Curator должны быть в том же пакете или модуле, что и Person. Кроме этого, у них обязательно должен быть один из модификаторов:

final, если класс запрещён к дальнейшему наследованию:
public final class Student extends Person{}
sealed, если наследование допустимо, но с заранее указанным списком наследников:
public sealed class Teacher extends Person permits MathTeacher, LanguageTeacher {}
non-sealed, когда для класса нужно снять любые ограничения по наследованию:
public non-sealed class Curator extends Person {}
У интерфейсов модификатор sealed:

sealed interface Person
   permits Student, Teacher, Curator {
}
С учётом record имплементировать интерфейс и записать класс Student:

public record Student(String name) implements Person{}
record по умолчанию final, ограничения класса sealed соблюдены.

Запечатанные классы помогают установить ограничение на число наследников, когда их набор определён и его не собираются часто менять. перечисление (enum), но sealed class гибче, потому что одни ветки наследования можно открыть для расширения, а другие ограничить только для использования.
