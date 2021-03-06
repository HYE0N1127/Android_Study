## ๐ป Room
- ์ค๋งํธํฐ ๋ด์ฅ DB์ ๋ฐ์ดํฐ๋ฅผ ์ ์ฅํ๊ธฐ ์ํ์ฌ ์ฌ์ฉ๋๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ
> ์ ํํ๋ SQLite์ ๊ธฐ๋ฅ์ ๋ชจ๋ ์ฌ์ฉํ  ์ ์๊ณ  DB๋ก์ ์ ๊ทผ์ ๋์์ฃผ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์๋๋ค.

## ๐ค ์ ์ฌ์ฉํ๋์?
- ์๋ณธ SQL์ ์ปดํ์ผ ์๊ฐ์ด ํ์คํ์ง ์์ต๋๋ค. ๋ง์ฝ ๋ฐ์ดํฐ์ ๋ณํ๊ฐ ์๊ธด๋ค๋ฉด ์๋์ผ๋ก ์ด๋ฅผ ์๋ฐ์ดํธ ํด์ผํฉ๋๋ค.<br/> 
์ด ๊ณผ์ ์์ ์๊ฐ์ด ๋ง์ด ์์๋๋ฉฐ, ์ค๋ฅ๊ฐ ์๊ธธ ์ ์์ต๋๋ค.
- SQL ์ฟผ๋ฆฌ์ ๋ฐ์ดํฐ ๊ฐ์ฒด๋ฅผ ๋ณํํ๊ธฐ ์ํด์๋ ๋ง์ ์์ฉ๊ตฌ ์ฝ๋(boilerplate code)๋ฅผ ์์ฑํด์ผ ํฉ๋๋ค.

## ๐ Room Library์ ๊ตฌ์ฑ ์์

![room](https://user-images.githubusercontent.com/63246501/131428955-e1c1efe6-0812-4de0-a79d-a3e0735438d5.png)

Room ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ ํฌ๊ฒ ์ธ๊ฐ์ง์ ํ๋ก ๊ตฌ์ฑ๋๋๋ฐ, <br/>์ด๋ ์ํฐํฐ(Entity), ๋ฐ์ดํฐ ์ ๊ทผ ๊ฐ์ฒด(DAO, Data Access Object), Room DataBase์๋๋ค.

### ๐ค Entity?
---
๋ฐ์ดํฐ๋ฒ ์ด์ค์ ํ์ด๋ธ์ ์๋ฐ, ๋๋ ์ฝํ๋ฆฐ์ ๊ฐ์ฒด์ผ๋ก ์ ์ํ ๊ฒ์๋๋ค.<br/>
@Entity ์ด๋ธํ์ด์์ ์ฌ์ฉํฉ๋๋ค.
```kotlin
@Entity(tableName = "todo")
class TodoListEntity(

    @PrimaryKey var idx: Int,
    var content: String,
    var title: String
    ) {
    constructor() : this(0, "", "")
}
```
Entity๋ฅผ ๋ง๋ค ๋์๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค ํ์ด๋ธ์ ๋ง๋ ๋ค๊ณ  ์๊ฐํ์๋ฉด ์ฝ์ต๋๋ค.<br/>
ํ์ง๋ง ๋ฐ์ดํฐ๋ฒ ์ด์ค ๊ฐ์ฒด ๋ฌด๊ฒฐ์ฑ ์ ์ฝ ์กฐ๊ฑด ๋ฑ์ ์ ๋ถ ๋ง์กฑํด์ผ ํ๊ธฐ์ ๊ทธ ๋ถ๋ถ์ ์ ๊ฒฝ์จ์ฃผ์์ผ ํฉ๋๋ค.

### ๐ค DAO?
---
DAO๋ Database Access Object์ ์ฝ์๋ก, ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ ๊ทผํ  ์ ์๋ ๋ฉ์๋๋ฅผ ์ ์ํด๋์ Interface์๋๋ค.<br/>
Entity์ ๋์ผํ๊ฒ Dao ์ด๋ธํ์ด์์ ์ฌ์ฉํด์ค๋๋ค.
```kotlin
@Dao
interface TodoListDAO {

    @Query("SELECT * FROM todo")
    fun getAllTodoList() : List<TodoListEntity>

    // OnConflictStrategy.REPLACE๋ฅผ ์ด์ฉํ์ฌ ๊ฐ์ด ์ค๋ณต์ผ๋ก ์กด์ฌํ๋ค๋ฉด ๊ทธ ๊ฐ ์์ ๋ฎ์ด์์
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    fun addTodo(todoEntity : TodoListEntity)
}
```
์ด๋ธํ์ด์์๋ ๋ค ๊ฐ์ง ์ข๋ฅ๊ฐ ์์ต๋๋ค.
- Insert : ๋ฐ์ดํฐ ์ฝ์
- Delete : ๋ฐ์ดํฐ ์ญ์ 
- Update : ๋ฐ์ดํฐ ์์ 
- Query : @Query ์ด๋ธํ์ด์ ๋ค์ SQL๋ฌธ์ ์ ์ด์ ์ฌ์ฉ ๋ฐฉ๋ฒ์ ์ ์<br/>
์์ ์ด๋ธํ์ด์๋ค์ ์ํฉ, ํจ์์ ๋ง๊ฒ ์ฌ์ฉํด์ฃผ์๋ฉด ๋ฉ๋๋ค.

### ๐ค Database?
---
Room์ ์ฒ์ ์ ํ์  ๋ถ๋ค์ด๋ผ๋ฉด <br/>
> '์๋ ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค์์ ๊ตฌ์ฑ์์๊ฐ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ธ๊ฑฐ์ผ'<br/>

๋ผ๊ณ  ์๊ฐํ์ค ์๋ ์์ ๊ฒ ๊ฐ์ต๋๋ค.<br/>
DataBase๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ฅผ ๊ด๋ฆฌํ๊ณ , ๊ฐ์ฒด๋ฅผ ๋ง๋ค๊ธฐ ์ํ์ฌ ์์ฑ๋๋ ์ถ์ ํด๋์ค์๋๋ค.
```kotlin
@Database(entities = [todo::class], version = 1)
// @Database(entities = arrayOf(todo::class, user::class), version = 1)
abstract class TodoDatabase: RoomDatabase() {
    abstract fun userDao(): UserDao
 
    companion object {
        private var instance: TodoDatabase? = null
 
        @Synchronized
        fun getInstance(context: Context): TodoDatabase? {
            if (instance == null) {
                synchronized(TodoDatabase::class){
                    instance = Room.databaseBuilder(
                        context.applicationContext,
                        TodoDatabase::class.java,
                        "todo-database"
                    ).build()
                }
            }
            return instance
        }
    }
}
```
> ๊ตฌ์กฐ๊ฐ ๋ฐ๋ ๋๋ง๋ค Version์ DataBase ํด๋์ค์์ ์๋ฐ์ดํธ๋ฅผ ํด์ฃผ์ด์ผ ํฉ๋๋ค.

RoomDataBase ํด๋์ค๋ฅผ ์์๋ฐ๊ณ , Database ์ด๋ธํ์ด์์ ์ฌ์ฉํฉ๋๋ค.<br/>
entities์๋ ์ ํฌ๊ฐ ๋ง๋ค์ด์ค Entity DataBase์ ์ด๋ฆ์ ๋ฃ์ด์ฃผ๋ฉด ๋ฉ๋๋ค.

> Room์ Retrofit2์ ๋ง์ฐฌ๊ฐ์ง๋ก ์์ฑ ๋น์ฉ์ด ๋ง์ด ๋ค๊ธฐ์ ๊ณต์๋ฌธ์์์ Singleton ํจํด์ ์ถ์ฒํ๊ณ  ์์ต๋๋ค. ํ์ฌ๋ ์ฌ์ฉํ์ง ์์ง๋ง, ์ ์ํ์ฌ ๋ด์ฃผ์๋ฉด ์ข๊ฒ ์ต๋๋ค.

databaseBuilder๋ผ๋ ๋ฉ์๋๋ฅผ ์ฌ์ฉํ์ฌ context์ DataBase ํด๋์ค, ๊ทธ๋ฆฌ๊ณ  ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ฅผ ์ ์ฅํ  ์ด๋ฆ์ ๋๊ฒจ์ฃผ๋ฉด ๋ฉ๋๋ค. ๋ค๋ฅธ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ด๋ฆ์ด ๊ฒน์น๋ฉด ์ฝ๋๊ฐ ๊ผฌ์ด๊ธฐ์ ์ฃผ์ํด์ฃผ์ธ์.

### ์ฌ์ฉ ๋ฐฉ๋ฒ
---
Room์ ๋ฌด์กฐ๊ฑด ๋ฉ์ธ ์ค๋ ๋ ๋ง๊ณ  ๋ฐฑ๊ทธ๋ผ์ด๋ ์ค๋ ๋์์ ์คํ์์ผ์ค์ผ ํฉ๋๋ค.<br/>
๋ฉ์ธ ์ค๋ ๋์์ ๋๋ ค์ค ๊ฒฝ์ฐ ์คํ ์๊ฐ์ด ์ค๋ ๊ฑธ๋ฆฌ๊ธฐ์ ์๋๋ก์ด๋ ์คํ๋์ค๊ฐ ํ๋ฅผ ๋ด๋ฉฐ ์คํ์ ์ค์ง์ํต๋๋ค.
```kotlin
    val r = Runnable {
         // ๋ฐ์ดํฐ๋ฅผ ์ฝ๊ณ  ์ธ ๋์๋ ์ฐ๋ ๋ ์ฌ์ฉ
        try {
            todoList = todoDB?.getTodoDao()?.getAllTodoList()!!             
            todoAdapter = TodoAdapter(this, todoList)
            todoAdapter.notifyDataSetChanged()

            rvTodoList.adapter = todoAdapter
            rvTodoList.layoutManager = LinearLayoutManager(this)
            rvTodoList.setHasFixedSize(true)

        } catch (e: Exception) {
            Log.d("tag", "Error - $e")
        }        
    }

        val thread = Thread(r)
        thread.start()
```
> ์์ ์ฝ๋๋ ๊ฐ๋จํ ํ์ต์ด๊ธฐ์ ์ค๋ ๋๋ฅผ ๋ง๋ค์ด์ ์ฒ๋ฆฌํด์ฃผ์์ต๋๋ค.<br/>
Android์๋ ๋น๋๊ธฐ ์ฒ๋ฆฌ๋ฅผ ์ํ ์ข์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ธ corutine, RXJava2 ๋ฑ์ด ์กด์ฌํ๊ธฐ์<br/>
์ด๋ฅผ ์ฌ์ฉํ์ฌ ์ฝ๋๋ฅผ ์ง๋ณด์๋ ๊ฒ๋ ์ข์ ๊ฒฝํ์ด ๋  ๊ฒ์๋๋ค.

์ด์์ผ๋ก Room์ ๋ํ ์ค๋ช์ ๋ง์น๊ฒ ์ต๋๋ค.<br/>
๋ด์ฉ์ ๋ํ ์ด์๋ ์ธ์ ๋ ํ์์๋๋ค ๐
