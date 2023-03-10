# 了解为什么房间是 SQLite - pt 的改型。2

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/check-out-why-room-is-a-retrofit-for-sqlite-pt.-2>

 在我们文章的第 1 部分中，我们介绍了 Room 的基本功能以及它与 retrieval 的相似之处(如果您还没有读过，[可以在这里](/web/20221205020441/https://www.netguru.com/blog/check-out-why-room-is-a-retrofit-for-sqlite-pt.-1)找到)。现在，我们将深入探讨实现。

## 我从哪里开始？

目前，Room 仍处于 alpha 版本，但它已经可以在谷歌的 maven 知识库中找到。为了拉它，加 Google 的 maven 库:

```
 allprojects {
    repositories {
        jcenter()
        maven { url 'https://maven.google.com' }
    }
} 
```

然后，向您的依赖项添加适当的工件:

```
 dependencies {
    ...
    compile "android.arch.persistence.room:runtime:1.0.0-alpha6"
    annotationProcessor "android.arch.persistence.room:compiler:1.0.0-alpha6"
    compile "android.arch.persistence.room:rxjava2:1.0.0-alpha6"
    ...
} 
```

就这样。我们现在准备好出发了！

## 我们数据库的新核心

扩展 RoomDatabase 的类是设置数据库属性和注册 DAOs 接口、实体和定制类型转换器的地方。

```
 @Database(version = 1, entities = {TaskDb.class, ChecklistItemDb.class})
@TypeConverters(LocalDateConverter.class)
public abstract class AppDatabase extends RoomDatabase {

    public abstract TasksDao tasksDao();

    public abstract ChecklistDao checklistDao();
} 
```

可以使用 Room.databaseBuilder(...).请记住，创建一个 AppDatabase 实例是相当昂贵的，所以您应该尝试使用这个对象的单例模式。在我们的例子中，我们使用了 dagger，它允许我们轻松地将单个实例注入到每个需要它的类中。

## 实体

创建一个新的实体，需要用@Entity 对类进行注释，并在@Database 注释中引用。基于您的类，将创建一个新表。默认情况下，Room 支持基本类型和字符串。对于其余的，我们将需要提供类型转换器。

包含类应该在@TypeConvertes 注释我们的 AppDatabase 类中引用。如果我们想为代表我们字段的列使用自定义名称，我们可以使用*@ column info(name = " some _ name ")*来覆盖它。这与我们在 Gson 中使用@SerializedName 注释或使用 Moshi 时使用*@ Json(name = " name ")*类似。

```
 @Entity(
        tableName = "check_list_item",
        indices = @Index("task_id"),
        foreignKeys = @ForeignKey(
                onUpdate = ForeignKey.CASCADE,
                onDelete = ForeignKey.CASCADE,
                entity = TaskDb.class,
                parentColumns = "id",
                childColumns = "task_id"
        ))
public class ChecklistItemDb {

    @PrimaryKey(autoGenerate = true)
    private final long id;

    @ColumnInfo(name = "task_id")
    private final long taskId;

    @ColumnInfo(name = "first_name")
    private final String name;

    ...
} 
```

与上面的例子一样，我们也可以定义一个外键，例如，它允许我们定义在 onDelete 中房间应该采取的动作。您可能已经注意到在一些 ORM 中缺少级联删除，但是在这里它又回来了，就好像它一直都在那里一样。

我们都喜欢 auto value，以及它如何让我们在创建值类时不用编写所有的样板文件。它似乎可以很好地处理房间实体，但目前不可能这样做，除非创建一个新的扩展(就像在 Moshi 或 Gson 中一样)。我不是唯一注意到这一点的人，而且已经有一张[的开放票了。](http://web.archive.org/web/20221205020441/https://issuetracker.google.com/issues/62408420) 前途一片光明。

## 型转换器

为了处理除了原语和字符串之外的类型，你需要定义类型转换器，将你的对象转换成 SQLite 支持的类型。创建它们相当容易，归结起来就是用@TypeConverter 注释一个转换器方法。在大多数情况下，您需要提供两个从对象类型到对象类型的转换方法，用于读取和写入数据。带注释的方法可以是静态方法，也可以是实例方法。在后一种情况下，包含类的实例将由 Room 创建。永远记得在扩展 RoomDatabase 的类中的@TypeConverters 注释中注册包含类型转换器的类。

```
 public class LocalDateConverter {

    private LocalDateConverter() {
        throw new AssertionError();
    }

    @TypeConverter()
    public static LocalDate fromLong(@Nullable Long epoch) {
        return epoch == null ? null : LocalDate.ofEpochDay(epoch);
    }

    @TypeConverter
    public static Long localDateToEpoch(@Nullable LocalDate localDate) {
        return localDate == null ? null : localDate.toEpochDay();
    }
}
view raw 
```

## 道斯

Dao 是你编写查询的地方，你会惊奇地发现现在编写查询变得简单多了。编译时查询检查做了一件了不起的工作，你会被告知每一个拼写错误或错误的表名或属性。

```
 @Dao
public interface TasksDao {

    @Insert
    long insertTask(TaskDb taskDb);

    @Update
    int updateTask(TaskDb taskDb);

    @Query("SELECT * FROM task WHERE is_done = 0")
    Flowable<List<TaskDb>> getToDoTasks();

    @Query("SELECT * FROM task WHERE is_done = 1")
    Flowable<List<TaskDb>> getDoneTasks();

    @Query("SELECT * FROM task WHERE is_done = 0 AND due_date < :localDate")
    Flowable<List<TaskDb>> getTaskWithDueDateBefore(LocalDate localDate);

    @Delete
    void deleteTask(TaskDb taskDb);
} 
```

得益于 RxJava 的支持，我们能够以一种真正反应式的方式利用它的全部能力。在订阅 getToDoTask 返回的可流动任务后，每次更新任务表时，都会发出一个与我们的查询匹配的新任务列表。正如所料，更新与任务有一对多关系的检查列表项不会导致任何排放。如果您需要这样的更新，您可以订阅检查列表项，或者将它们与任务结合起来，然后采取适当的措施。目前，Room 支持 Flowable、Publisher、Maybe、Single 或 Entity 类型的查询，以及 void 或 int 类型的删除、插入和更新。如果我们也想将 RxJava 用于这些操作，我们可以使用 Completable.fromAction(...)或 Single.fromCallable(...)，或者甚至在我们的 DAO 接口中，通过将它转换成一个抽象类并为它提供我们的自定义实现:

```
 @Dao
public abstract class TasksDao {

    @Insert
    public abstract long insertTask(TaskDb taskDb);

    public Completable insertTaskCompletable(TaskDb taskDb) {
        return Completable.fromAction(() -> insertTask(taskDb));
    }

} 
```

正如我们在第 1 部分中提到的，在某些情况下，改造和房间之间可以共享数据访问接口。由于我们的示例应用程序完全离线工作，我们在一个不同的项目中测试了这个解决方案。以下是我们在改造和房间之间共享的界面外观:

```
 @Dao
public interface UserSource {

    @GET("api/")
    @android.arch.persistence.room.Query("SELECT * FROM user LIMIT :amount")
    Flowable<List<User>> getUsersList(@Query("amount") int amount);
} 
```

这种方法可能在一些琐碎的情况下有效，但它也有自己的缺点，例如 reform 和 Room 的不同行为(Room 寄存器用于表格更改，而 reform 实际上将返回单个值)，对象之间更复杂关系的问题，以及目前没有使用 autovalue 的默认方式。

## 交易、插入、删除

如前所述，Room 不处理对象引用，开发人员有责任正确处理它们并确保数据的一致性。为了实现这一点，使用了事务。如果任何插入失败，数据将被回滚。

```
 public Completable saveNewTask(Task task) {
    return Completable.fromAction(() -> {
                TaskDb taskDb = TaskMapper.toTaskDb(task);
                appDatabase.beginTransaction();
                try {
                    long taskId = appDatabase.tasksDao().insertTask(taskDb);
                    List<ChecklistItemDb> checklistItemDbs = ChecklistItemMapper.toChecklistItemDbList(taskId, task.getChecklistItemList());
                    appDatabase.checklistDao().insertAll(checklistItemDbs);
                    appDatabase.setTransactionSuccessful();
                } finally {
                    appDatabase.endTransaction();
                }
            }
    );
} 
```

## 有东西不见了……

假设您想要删除所有插入的任务，但是您不想查询每个任务，然后将它们传递给用@Delete 注释的方法。如果没有@DeleteAll 注释可以吗？答案是肯定的，因为您仍然可以通过 Room 访问 SQLite 数据库。您可以通过编译自己的语句并像下面这样执行它们来进行自己的查询:

```
 public class TasksRepository {

    private final AppDatabase appDatabase;

    @Inject
    public TasksRepository(AppDatabase appDatabase) {
        this.appDatabase = appDatabase;
    }

    private void deleteAllTasks() {
        appDatabase.compileStatement("DELETE FROM task").execute();
    }

    ...
} 
```

## 结论

我对与 Room 的无缝合作感到非常惊讶。SQL 解析允许在编译时发现错误，这是一项了不起的工作。Room 还可以更好地防止开发人员可能引入的性能问题——它在警告中给出提示，或者完全阻止主线程上的任何操作。尽管 Room 仍处于测试阶段，但我们没有发现任何关键问题，而且已经可用的功能运行得非常好。

我们绝对期待一个稳定版本的发布，因为 Room 似乎是一个很棒的工具。如果您想了解更多或查看本文中的实例，这里有一个资源库 [。](http://web.archive.org/web/20221205020441/https://github.com/netguru/android-todo-app)