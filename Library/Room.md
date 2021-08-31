## 💻 Room
- 스마트폰 내장 DB에 데이터를 저장하기 위하여 사용되는 라이브러리
> 정확히는 SQLite의 기능을 모두 사용할 수 있고 DB로의 접근을 도와주는 라이브러리입니다.

## 🤔 왜 사용하나요?
- 원본 SQL은 컴파일 시간이 확실하지 않습니다. 만약 데이터에 변화가 생긴다면 수동으로 이를 업데이트 해야합니다.<br/> 
이 과정에서 시간이 많이 소요되며, 오류가 생길 수 있습니다.
- SQL 쿼리와 데이터 객체를 변환하기 위해서는 많은 상용구 코드(boilerplate code)를 작성해야 합니다.

## 📃 Room Library의 구성 요소

![room](https://user-images.githubusercontent.com/63246501/131428955-e1c1efe6-0812-4de0-a79d-a3e0735438d5.png)

Room 라이브러리는 크게 세가지의 틀로 구성되는데, <br/>이는 엔티티(Entity), 데이터 접근 객체(DAO, Data Access Object), Room DataBase입니다.

### 🤔 Entity?
---
데이터베이스의 테이블을 자바, 또는 코틀린의 객체으로 정의한 것입니다.<br/>
@Entity 어노테이션을 사용합니다.
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
Entity를 만들 때에는 데이터베이스 테이블을 만든다고 생각하시면 쉽습니다.<br/>
하지만 데이터베이스 개체 무결성 제약 조건 등을 전부 만족해야 하기에 그 부분을 신경써주셔야 합니다.

### 🤔 DAO?
---
DAO란 Database Access Object의 약자로, 데이터베이스에 접근할 수 있는 메서드를 정의해놓은 Interface입니다.<br/>
Entity와 동일하게 Dao 어노테이션을 사용해줍니다.
```kotlin
@Dao
interface TodoListDAO {

    @Query("SELECT * FROM todo")
    fun getAllTodoList() : List<TodoListEntity>

    // OnConflictStrategy.REPLACE를 이용하여 값이 중복으로 존재한다면 그 값 위에 덮어씌움
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    fun addTodo(todoEntity : TodoListEntity)
}
```
어노테이션에는 네 가지 종류가 있습니다.
- Insert : 데이터 삽입
- Delete : 데이터 삭제
- Update : 데이터 수정
- Query : @Query 어노테이션 뒤에 SQL문을 적어서 사용 방법을 정의<br/>
위의 어노테이션들을 상황, 함수에 맞게 사용해주시면 됩니다.

### 🤔 Database?
---
Room을 처음 접하신 분들이라면 <br/>
> '아니 왜 데이터베이스에서 구성요소가 데이터베이스인거야'<br/>

라고 생각하실 수도 있을 것 같습니다.<br/>
DataBase는 데이터베이스를 관리하고, 객체를 만들기 위하여 생성되는 추상 클래스입니다.
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
> 구조가 바뀔 때마다 Version을 DataBase 클래스에서 업데이트를 해주어야 합니다.

RoomDataBase 클래스를 상속받고, Database 어노테이션을 사용합니다.<br/>
entities에는 저희가 만들어준 Entity DataBase의 이름을 넣어주면 됩니다.

> Room은 Retrofit2와 마찬가지로 생성 비용이 많이 들기에 공식문서에서 Singleton 패턴을 추천하고 있습니다. 현재는 사용하지 않지만, 유의하여 봐주시면 좋겠습니다.

databaseBuilder라는 메서드를 사용하여 context와 DataBase 클래스, 그리고 데이터베이스를 저장할 이름을 넘겨주면 됩니다. 다른 데이터베이스와 이름이 겹치면 코드가 꼬이기에 주의해주세요.

### 사용 방법
---
Room은 무조건 메인 스레드 말고 백그라운드 스레드에서 실행시켜줘야 합니다.<br/>
메인 스레드에서 돌려줄 경우 실행 시간이 오래 걸리기에 안드로이드 스튜디오가 화를 내며 실행을 중지시킵니다.
```kotlin
    val r = Runnable {
         // 데이터를 읽고 쓸 때에는 쓰레드 사용
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
> 위의 코드도 간단한 학습이기에 스레드를 만들어서 처리해주었습니다.<br/>
Android에는 비동기 처리를 위한 좋은 라이브러리인 corutine, RXJava2 등이 존재하기에<br/>
이를 사용하여 코드를 짜보시는 것도 좋은 경험이 될 것입니다.

이상으로 Room에 대한 설명을 마치겠습니다.<br/>
내용에 대한 이슈는 언제나 환영입니다 😊
