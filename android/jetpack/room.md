# Room
- Room 라이브러리는 SQLite DB의 최상위에 있는 추상적인 레이어
- 단순한 함수 호출만으로 데이터베이스를 쉽게 이용할 수 있도록 해준다

# Entity
- 데이터베이스에 저장할 속성을 가지고 있는 하나의 객체 혹은 개념
- 데이터베이스의 테이블은 entity class로 정의되며, 각 행은 해당 class의 instance를 나타냄
- Entity class는 Room 라이브러리에게 어떻게 정보를 표시하고 데이터베이스와 상호작용 할 것인지 나타낸다
- Entity class는 annotated data class로 정의해야 함

```kotlin
@Entity(tableName = "daily_sleep_quality_table")
data class SleepNight(
    @PrimaryKey(autoGenerate = true)
    var nightId: Long = 0L,

    @ColumnInfo(name = "start_time_milli")
    val startTimeMilli: Long = System.currentTimeMillis(),

    @ColumnInfo(name = "end_time_milli")
    var endTimeMilli: Long = startTimeMilli,

    @ColumnInfo(name = "quality_rating")
    var sleepQuality: Int = -1
)
```

# Query
- 데이터베이스 하나의 테이블 혹은 여러 테이블의 조합의 데이터 혹은 정보에 대한 요청이거나, 데이터에 대한 작업 요청

# Dao 구현
- DAO는 데이터베이스 삽입, 삭제, 수정에 대한 기본 함수를 제공
- query는 @Query annotation을 사용하여 정의

```kotlin
@Dao
interface SleepDatabaseDao {
    @Insert
    fun insert(night : SleepNight)

    @Update
    fun update(night : SleepNight)

    @Query("SELECT * from daily_sleep_quality_table WHERE nightId = :key")
    fun get(key : Long) : SleepNight?

    @Query("DELETE FROM daily_sleep_quality_table")
    fun clear()

    @Query("SELECT * FROM daily_sleep_quality_table ORDER BY nightId DESC LIMIT 1")
    fun getTonight(): SleepNight?

    @Query("SELECT * FROM daily_sleep_quality_table ORDER BY nightId DESC")
    fun getAllNights() : LiveData<List<SleepNight>>
}
```

# 데이터베이스 구현
- Entity와 DAO 인터페이스를 이용하는 Room database를 생성
- 추상 클래스로서, '@Database' annotation과 함께 정의
- 데이터베이스는 여러 인스턴스가 필요 없으므로 싱글톤 패턴을 사용한다
- UI스레드에서 이용하는 것은 기본적으로 권장되지 않으나 사용을 위해서는 최초 빌드시 'allowMainThreadQueries()'를 호출해야함
- 코루틴 사용시 DAO의 methods를 suspend로 변경
> 단 LiveData의 경우 suspend로 선언할 필요가 없다

```kotlin
@Database(entities = [SleepNight::class], version = 1, exportSchema = false) // Entity 정의, Schema 변경 시 version up
abstract class SleepDatabase : RoomDatabase() {
   abstract val sleepDatabaseDao: SleepDatabaseDao // DAO 선언. 여러 개의 DAO를 가질 수 있음.
}

companion object {
    @Volatile // Multi-Thread safe 하도록 Volatile 선언
    private var INSTANCE: SleepDatabase? = null

    fun getInstance(context: Context): SleepDatabase {
        synchronized(this) {
            var instance = INSTANCE

            if (instance == null) {
                instance = Room.databaseBuilder(
                    context.applicationContext,
                    SleepDatabase::class.java,
                    "sleep_history_database" // DB 파일 이름
                )
                .fallbackToDestructiveMigration()
                .build()
                INSTANCE = instance
            }
            return instance
        }
    }
}
```
