Object string = "this is string!";

if(string instanceof String){
   String realString = (String) string;
   System.out.println(realString);
}

с Java 16 присвоение не требуется. Значение переменной задать в выражении:

if(string instanceof String realString){
   System.out.println(realString);
}

Если условие проверки не выполняется, оператор instanceof не ограничивается фигурными скобками внутри условия if, а проверяет код дальше:

Object object = 23;

if (!(object instanceof Number number)) {
   throw new IllegalArgumentException("this is not a Number!");
}

проверка !(object instanceof Number number) выдаёт результат false, и после выхода из if использовать number для реализации своей логики.


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

У класса два поля, но много бойлерплейтного кода(Бойлерплейт — код, вставляют в приложение почти без изменений. Иногда одни и те же шаблоны кода копируют в разные части приложения).

var student = new Student("Alex", CourseType.MATH);
var studentSame = new Student("Alex", CourseType.MATH);

System.out.println(studentSame.equals(student));
System.out.println(studentSame.hashCode() == student.hashCode());

До класс record, помогал плагин и библиотека Lombok — указать аннотациями, какие методы сгенерировать для класса на этапе компиляции. С ней класс:

@Data
@RequiredArgsConstructor
public class Student {
    private final String name;
    private final CourseType courseType;
}

@Data генерирует геттеры, сеттеры, equals(), hashCode() и toString(). @RequiredArgsConstructor создаёт конструктор с итоговыми параметрами класса и добавляет аргументы в порядке объявления. лишней зависимости и плагина для среды разработки.

Если запустить тестовый код, формат преобразования в строку отличается, но в остальном всё работает одинаково:

Класс record избавляет от бойлерплейтного кода. не нужны внешние плагины, достаточно встроенных возможностей языка. создать класс, указать только два поля — конструктор, геттеры, equals(), hashCode() и toString() уже включены в класс.

public record Student(String name, CourseType courseType) {}
Запустим тестовый код и увидим тот же результат:

У геттеров больше нет приставки get., к методу обращаемся по имени переменной:

student.name(); //Alex
student.courseType(); //MATH

public record Student(String name, CourseType courseType) {

   public Student {
       if (name == null || name.isBlank()) {
           throw new IllegalArgumentException();
       }
   }
}

public record Student(String name, CourseType courseType) {

   public Student(String name){
       this(name, CourseType.MATH);
   }
}

все объявленные поля получают модификатор final (Если класс объявили с модификатором final, от него нельзя наследоваться.);
все поля класса объявляются в заголовке, дополнительные объявить нельзя:

//ошибка компиляции:
public record Student(String name, CourseType courseType) {
	private int id;
}

можно объявлять static-поля класса;
класс record неявно объявлен как final, поэтому его нельзя наследовать;
он не может быть абстрактным и наследовать другие классы;
для конструктора можно использовать проверку аргументов;
можно переопределить стандартные методы — геттеры, toString(), equals() и hashCode();
можно добавлять статические и нестатические методы.

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

Sealed class «запечатанный класс». В этом классе сразу объявить список классов-наследников, кроме них наследников быть не может. похоже на enum, только в разрезе наследования.

sealed class Person permits Student, Teacher, Curator {}
Классы Student, Teacher и Curator должны быть в том же пакете или модуле, что и Person. у них обязательно должен быть один из модификаторов:

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

Запечатанные классы помогают установить ограничение на число наследников, когда их набор определён и его не собираются часто менять. 
перечисление (enum), но sealed class гибче, одни ветки наследования можно открыть для расширения, а другие ограничить только для использования.
