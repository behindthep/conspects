Task — абстрактное описание задачи: у задачи есть название, статус, методы для выполнения.
TaskRepository — абстракция для хранения задач (не важно, где — в памяти, файле, базе данных).
TaskService — бизнес-логика: добавление задач, поиск, выполнение.

// Слой бизнес-логики
public abstract class Task {
    private String title;
    private boolean completed;

    public Task(String title) {
        this.title = title;
        this.completed = false;
    }

    public abstract void complete();

    public String getTitle() { return title; }
    public boolean isCompleted() { return completed; }

    protected void setCompleted(boolean completed) { this.completed = completed; }
}

// Слой хранения данных (абстракция)
public interface TaskRepository {
    void save(Task task);
    Task findByTitle(String title);
    List<Task> findAll();
}


public class InMemoryTaskRepository implements TaskRepository {
    private List<Task> tasks = new ArrayList<>();

    @Override
    public void save(Task task) {
        tasks.add(task);
    }

    @Override
    public Task findByTitle(String title) {
        for (Task task : tasks) {
            if (task.getTitle().equals(title)) {
                return task;
            }
        }
        return null;
    }

    @Override
    public List<Task> findAll() {
        return new ArrayList<>(tasks);
    }
}

public class TaskService {
    private TaskRepository repository;

    public TaskService(TaskRepository repository) {
        this.repository = repository;
    }

    public void addTask(Task task) {
        repository.save(task);
    }

    public void completeTask(String title) {
        Task task = repository.findByTitle(title);
        if (task != null) {
            task.complete();
        } else {
            System.out.println("Задача не найдена: " + title);
        }
    }

    public void showAllTasks() {
        for (Task task : repository.findAll()) {
            System.out.println(task.getTitle() + " — " + (task.isCompleted() ? "выполнена" : "не выполнена"));
        }
    }
}

хранить задачи в базе данных, а не в памяти — реализуем новый класс DatabaseTaskRepository, не переписывая бизнес-логику и UI.

Абстрактный класс должен отвечать только за одну область поведения. Не смешивать, например, методы для хранения и методы для бизнес-логики в одном абстрактном классе.

Жёсткая связь между слоями. Если слой бизнес-логики напрямую зависит от конкретной реализации хранилища (например, использует new InMemoryTaskRepository() внутри себя), при замене хранилища придётся переписывать весь код. Используйте абстракции (интерфейсы, абстрактные классы) для ослабления связей.

менеджер задач на новый уровень, сделав его модульным и легко расширяемым. три основных слоя взаимодействия: UserInterface (то, с чем взаимодействует пользователь), TaskService (бизнес-логика, управляющая задачами) и InMemoryTaskRepository (конкретная реализация хранилища задач в памяти).

Класс UserInterface должен содержать метод void addAndCompleteTask(String title), который будет отвечать за полный цикл: он добавляет задачу в систему и тут же отмечает её как выполненную, используя для этого TaskService.

связать все эти классы между собой, используя внедрение зависимостей через конструкторы. UserInterface не должен иметь прямого представления о том, как реализован репозиторий; он должен "знать" только о TaskService. TaskService должен работать не с конкретной реализацией InMemoryTaskRepository, а лишь с интерфейсом TaskRepository, обеспечивая максимальную гибкость.

работу многослойной архитектуры, добавив и сразу же выполнив какую-либо задачу через метод addAndCompleteTask() в UserInterface.

три слоя: UserInterface (интерфейс пользователя), TaskService (слой бизнес-логики) и InMemoryTaskRepository (реализация хранилища задач в памяти).
Должен быть определен интерфейс TaskRepository, а класс InMemoryTaskRepository должен реализовывать этот интерфейс.
Класс TaskService должен зависеть только от интерфейса TaskRepository, а не от его конкретной реализации.
UserInterface должен принимать TaskService через конструктор, а TaskService — TaskRepository через конструктор.
Класс UserInterface не должен иметь прямых ссылок на репозиторий или его реализацию; он работает только с TaskService.
В классе UserInterface должен быть реализован публичный метод void addAndCompleteTask(String title), который добавляет задачу и тут же помечает её как выполненную через TaskService.
В основной части программы необходимо создать экземпляры всех слоёв, корректно связать их друг с другом и вызвать addAndCompleteTask() для какой-либо задачи.
Архитектура должна позволять легко заменить реализацию TaskRepository без изменений в TaskService и UserInterface.

import java.util.HashMap;
import java.util.Map;

 * Демонстрация многослойной архитектуры с внедрением зависимостей через конструкторы.
 * Слои: UserInterface -> TaskService -> TaskRepository (реализация InMemoryTaskRepository).

public class Solution {
    public static void main(String[] args) {
        // Создаем реализацию репозитория (хранилище в памяти)
        TaskRepository repository = new InMemoryTaskRepository(); // Можно легко заменить на другую реализацию

        // Внедряем репозиторий в сервис (через конструктор)
        TaskService service = new TaskService(repository);

        // Внедряем сервис в пользовательский интерфейс (через конструктор)
        UserInterface ui = new UserInterface(service);

        // Демонстрация работы: добавляем и тут же помечаем задачу выполненной
        ui.addAndCompleteTask("Выучить внедрение зависимостей через конструктор");
    }
}

 * Интерфейс репозитория задач — контракт для любых реализаций хранилища.
 * TaskService зависит только от этого интерфейса (инверсия зависимостей).
   
interface TaskRepository {
    // Создает и сохраняет новую задачу, возвращает её id
    int add(String title);

    // Помечает задачу выполненной
    void markCompleted(int id);

    // Получает задачу по id, либо null, если не найдена
    Task getById(int id);
}

 * Хранит задачи в обычной HashMap.

class InMemoryTaskRepository implements TaskRepository {
    private final Map<Integer, Task> storage = new HashMap<>();
    private int sequence = 1; // Генератор id (простой счетчик)

    @Override
    public int add(String title) {
        int id = sequence++;
        storage.put(id, new Task(id, title));
        return id;
    }

    @Override
    public void markCompleted(int id) {
        Task task = storage.get(id);
        if (task != null) {
            task.setCompleted(true);
        }
    }

    @Override
    public Task getById(int id) {
        return storage.get(id);
    }
}


 * Сервис — слой бизнес-логики. Работает только через интерфейс TaskRepository.
 * Не знает, как устроено хранилище.

class TaskService {
    private final TaskRepository repository;

    // Внедрение зависимости через конструктор — сервис получает абстракцию (интерфейс), а не конкретику
    public TaskService(TaskRepository repository) {
        this.repository = repository;
    }

    public int addTask(String title) {
        return repository.add(title);
    }

    public void completeTask(int id) {
        repository.markCompleted(id);
    }

    public Task getTask(int id) {
        return repository.getById(id);
    }
}


 * Пользовательский интерфейс. Общается только с TaskService.
 * Не имеет прямых ссылок на репозиторий (и не знает о его реализации).

class UserInterface {
    private final TaskService service;

    // Внедрение зависимости через конструктор — UI получает сервис
    public UserInterface(TaskService service) {
        this.service = service;
    }


     * Добавляет задачу и сразу же помечает её как выполненную.
     * Вся работа идет через TaskService — UI не знает про хранилище.

    public void addAndCompleteTask(String title) {
        int id = service.addTask(title);     // добавляем задачу через сервис
        service.completeTask(id);            // сразу помечаем выполненной
        Task task = service.getTask(id);     // получаем задачу для отображения результата

        // Демонстрация результата на экране:
        System.out.println("Добавлено и выполнено: " + task);
    }
}


 * Модель задачи: id, заголовок и флаг выполнения.

class Task {
    private final int id;
    private final String title;
    private boolean completed;

    public Task(int id, String title) {
        this.id = id;
        this.title = title;
        this.completed = false;
    }

    public int getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }

    public boolean isCompleted() {
        return completed;
    }

    // Сеттер оставляем простым: в учебной задаче нет сложной валидации
    public void setCompleted(boolean completed) {
        this.completed = completed;
    }

    @Override
    public String toString() {
        // Небольшое красивое представление задачи
        return "#" + id + " [" + (completed ? "✓" : " ") + "] " + title;
    }
}



Пример с внедрением зависимости

public interface NotificationSender {
    void send(String message);
}

public class EmailNotificationSender implements NotificationSender {
    @Override
    public void send(String message) {
        System.out.println("Отправка Email: " + message);
    }
}

public class SmsNotificationSender implements NotificationSender {
    @Override
    public void send(String message) {
        System.out.println("Отправка SMS: " + message);
    }
}

// Класс, который использует NotificationSender
public class NotificationService {
    private final NotificationSender sender;

    public NotificationService(NotificationSender sender) {
        this.sender = sender;
    }

    public void notifyUser(String message) {
        sender.send(message);
    }
}


Если вы везде используете конкретные классы, а не интерфейсы, то любое изменение реализации потребует переписывать код во многих местах. Всегда старайтесь программировать «на уровне интерфейсов».

Интерфейс должен быть компактным и отвечать за одну зону ответственности.
интерфейсы для подмены зависимостей в тестах
Используйте внедрение зависимостей (DI)
