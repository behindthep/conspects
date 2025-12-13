Для возможности сортировки объектов в коллекциях наследниках List в Java статический метод класса java.util.Collections.
сортировать элементы таких классов как ArrayList, LinkedList, CopyOnWriteArrayList и других классов, имплементирующих интерфейс List.
если у вас есть список из строк:

[z, b, c, a, k, z]

после сортировки получите в списке порядок:

[a, b, c, k, z, z]

в списке находятся объекты классов, которые известно как сравнить, достаточно вызвать метод sort() и передать туда список. в списке элементы поменяют порядок и отсортированы в порядке возрастания

//создание списка на основе массива
var stringList = Arrays.asList("z", "b", "c", "a", "k", "z");

[z, b, c, a, k, z]

//сортировка списка в порядке возрастания
Collections.sort(stringList);

[a, b, c, k, z, z]

сортировать множество стандартных классов, как String, Integer, Double, Character и других.
без доп параметров возможно отсортировать список из любых элементов, классы которых имплементируют интерфейс сравнения Comparable<T>.

сортировать элементы в обратном порядке. передадим доп аргумент в метод сортировки:

//создание списка на основе массива
var stringList = Arrays.asList("z", "b", "c", "a", "k", "z");

[z, b, c, a, k, z]

//сортировка списка в обратном направлении
Collections.sort(stringList, Collections.reverseOrder());

[z, z, k, c, b, a]

Добавляем возможность сортировки своих классов
Если стандартные классы уже готовы к сортировке, напишем свой класс, Java не знает как есть сравнивать с объектами этого же класса.

научить сравнивать объекты есть два варианта:
создать класс на основе Comparator<T> и там прописать правила сравнения в методе int compare(T o1, T o2). Полученный объект из класса использовать всегда, когда нам надо сортировать объекты. сортировать объекты по разным правилам и можем использовать нужный нам класс Comparator.
добавить в класс (являющимся, элементом списка) имплементацию интерфейса Comparable<T> и прописать правила сравнения в методе int compareTo(T o). не потребуется указывать каждый раз компаратор, данное правило сравнение будет по-умолчанию для этого объекта.
Оба метода возвращают целое число, которое интерпретируется так:

число больше 0 -> объект с которым сравнивают больше текущего
число равно 0 -> объекты одинаковые
число меньше 0 -> объект с которым сравнивают меньше текущего

Создадим класс для студента:

class Student {
    private final String name;
    private final double avgMark;

    public Student(String name, double avgMark) {
        this.name = name;
        this.avgMark = avgMark;
    }

    @Override
    public String toString() {
        return "{n='" + name + '\'' + ", m=" + avgMark + '}';
    }
}

все параметры задаются в конструкторе, и используются значения только для печати данных при вызове toString, поможет в визуализации результата.
отсортировать список из студентов:

var ivan = new Student("Иван", 4.3);
var olga = new Student("Ольга", 3.8);
var eugene = new Student("Женя", 4.9);
var studentList = Arrays.asList(ivan, olga, eugene);

System.out.println(studentList);
//сортировка списка
Collections.sort(studentList);
System.out.println(studentList);

Такой код не скомпилируется, sort() не просто ожидает список, важно, чтобы элемент списка был наследником Comparable:

public static <T extends Comparable<? super T>> void sort(List<T> list) {
 list.sort(null);
}

Использование Comparable<T>
Для создания возможности сортировки, научить сравнить объекты с другими такого-же типа. реализация использоваться по-умолчанию при сравнении объектов одного класса.

Имплементируем Comparable интерфейс, и реализуем метод compareTo:

class Student implements Comparable<Student> {
    private final String name;
    private final double avgMark;

    public Student(String name, double avgMark) {
        this.name = name;
        this.avgMark = avgMark;
    }

    @Override
    public String toString() {
        return "{n='" + name + '\'' + ", m=" + avgMark + '}';
    }

    @Override
    public int compareTo(Student o) {
        return name.compareTo(o.name);
    }
}

внутри метод сравнить две строки, у String есть реализация Comparable - можем ее использовать.

В коде опущены части, с проверкой на null объектов o и полей класса.

var ivan = new Student("Иван", 4.3);
var olga = new Student("Ольга", 3.8);
var eugene = new Student("Женя", 4.9);

var studentList = Arrays.asList(ivan, olga, eugene);

[{n='Иван', m=4.3}, {n='Ольга', m=3.8}, {n='Женя', m=4.9}]

//сортировка списка
Collections.sort(studentList);

[{n='Женя', m=4.9}, {n='Иван', m=4.3}, {n='Ольга', m=3.8}]

список отсортирован по полю name.
сложные условия сравнения, учитывать требование для успешной сортировки - два объекта, сколько бы мы их не сравнивали - должны всегда давать одинаковый результат.

Использование Comparator<T>
сортировать студентов не по имени, а по средней оценке? оставить возможность сортировать по имени, которое должна использоваться по умолчанию для создания различных документов.
отдельный класс Comparator, которые хранит логику сравнения объектов и при сортировке, использовать правило, объект класса Comparator.
добавим в класс Student геттеры, уже необходимо использовать данные класса в классе компаратора.

class Student implements Comparable<Student> {
    private final String name;
    private final double avgMark;

    public Student(String name, double avgMark) {
        this.name = name;
        this.avgMark = avgMark;
    }

    @Override
    public String toString() {
        return "{n='" + name + '\'' + ", m=" + avgMark + '}';
    }

    @Override
    public int compareTo(Student o) {
        return name.compareTo(o.name);
    }

    public String getName() {
        return name;
    }

    public double getAvgMark() {
        return avgMark;
    }
}

создадим класс Comparator, тип для сравнения Student:

class ComparatorByAvgMark implements Comparator<Student> {
    @Override
    public int compare(Student o1, Student o2) {
        return Double.compare(o1.getAvgMark(), o2.getAvgMark());
    }
}

готовый метод для сравнения стандартного класса Double, не выдумывать свои реализации, а использовать сущ.
опущены проверки на null объектов o1, o2.
перегруженный метод Collections.sort(), который принимает компаратор:

var ivan = new Student("Иван", 4.3);
var olga = new Student("Ольга", 3.8);
var eugene = new Student("Женя", 4.9);

var studentList = Arrays.asList(ivan, olga, eugene);

[{n='Иван', m=4.3}, {n='Ольга', m=3.8}, {n='Женя', m=4.9}]

//сортировка списка c использованием компаратора
Collections.sort(studentList, new ComparatorByAvgMark());

[{n='Ольга', m=3.8}, {n='Иван', m=4.3}, {n='Женя', m=4.9}]

сортировка по возрастанию средней оценки студента.
обратную сортировку, высокие оценки должны быть в начале списка. изменить поведение компаратора, у компаратора есть метод reversed():

Collections.sort(studentList, new ComparatorByAvgMark().reversed());

[{n='Женя', m=4.9}, {n='Иван', m=4.3}, {n='Ольга', m=3.8}]

можно создавать цепочки. сортируем по оценкам, а если оценки одинаковые, то по имени.
реализовать не создавая отдельного класса, а воспользоваться функцией:

Collections.sort(studentList,
        new ComparatorByAvgMark().reversed()
                .thenComparing(Student::getName));

оценки в порядке убывания, а внутри одной средней оценки, студенты по имени в порядке возрастания.

sort() у списка
Кроме использования метода Collections.sort(), вызывать похожий метод у самого списка List.sort(). Метод принимает аргумент - компаратор.

studentList.sort(new ComparatorByAvgMark());
