 делить сложное на простое, а ещё лучше — на уровни абстракции.

на каждом этаже своя жизнь, но все этажи связаны между собой. слои (уровни):
Пользовательский интерфейс (UI)
Бизнес-логика — правила и процессы, которые реализуют суть приложения.
Доступ к данным (DAO, Repository) — работа с бд или файлами.

Каждый слой работает с абстракциями, не зная деталей других слоёв. бизнес-логике не важно, как реализован интерфейс пользователя или как хранятся данные — ей важно, что есть методы типа saveOrder() или findUserById().


Выделяем абстракции
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


Реализация TaskRepository

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

Реализация TaskService

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

хранить задачи в базе данных, а не в памяти — просто реализуем новый класс DatabaseTaskRepository, не переписывая бизнес-логику и UI.

