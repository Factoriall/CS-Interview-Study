# Android

- [Kotlin/Java](#Kotlin/Java)
- [Manifest 파일](#Manifest-파일)
- [4대 컴포넌트](#4대-컴포넌트)
- [LifeCycle](#LifeCycle)
  + Activity
  + Fragment
  + Service
- [Coroutine](#Coroutine)
- [Room](#Room)

## Kotlin/Java

## Manifest 파일
- 앱에 대한 필수적인 정보를 안드로이드 Build Tool 및 Android OS, 그리고 구글 플레이에 제공하는 역할

### 필수 선언 정보
1. 앱의 패키지 이름
- 용도
  + 앱 리소스에 접근하는데 사용되는 R클래스 네임 스페이스
  + Manifest 파일 내에 선언된 상대 경로에 적용
    * <activity android:name=".MainActivity> 라고 선언했다면 이는 "com.ready.example.MainActivity" 를 가리킴
2. 앱 컴포넌트들
- Activity
- Service
- Broadcast Receiver
- Content Provider
3. 권한
- 특정 시스템 기능을 사용할 때 요청하는 것
4. 디바이스 특징
- 앱이 필요로 하는 하드웨어 또는 소프트웨어 특징 명시
- <uses-feature> 태그를 사용하면 명시
- ex) 카메라 있는 기기에서만 다운로드 될 수 있게 설정 가능

## 4대 컴포넌트

![components](../image/android_components.png)

- 각 컴포넌트의 경우 독립적인 형태로 존재하며 고유의 기능을 가짐
- 각 컴포넌트들은 Intent를 통해 상호작용을 함

### Activity
- **사용자와 상호작용**을 담당하는 인터페이스
- 사용자가 애플리케이션과 상호작용하는 단일화면(UI)
- 생명 주기(Life Cycle) 관련 메서드들을 재정의 가능

  + Intent를 통해 다른 액티비티 호출 가능
  + 2개 이상의 액티비티 동시 Display 할 수 없음
  + 1개 이상의 View/ViewGroup을 포함
  + 애플리케이션에 무조건 1개 이상 존재해야 함
  + Fragment를 추가해서 액티비티 내의 화면을 분할시킬 수 있음

### Service
- **Background에서 특정 작업을 처리**하기 위해 사용하는 컴포넌트
- 음원 스트리밍 노래를 들으면서 작업을 한다던가 등의 2개 이상의 작업을 동시에 해야 할 때 서비스 사용
- **UI Thread에서 동작**, 별도 Thread를 생성해서 작업을 처리해야함
  + 네트워크 연동 가능
  + 별도 UI를 가지지 않고 Notification을 통해서 UI 표현 가능
  + 애플리케이션이 종료되도 이미 시작이 된 서비스는 Background에서 계속 동작

### Broadcast Receiver
- **안드로이드 OS로부터 발생하는 다양한 이벤트를 받아와 핸들링**하는 컴포넌트
- 앱 초기화, 네트워크 끊김, 문자 수신 등의 정보를 처리해야 할 필요가 있을 때 동작
- 이러한 메세지들을 받기 위해 리시버 구현 시 특정 이벤트 처리 가능

  + 대부분 UI가 없음
  + 디바이스의 특수 상황에 대응하기 위해 사용
  + 특정 상황을 제외하고 시스템에서 시작

### Content Provider
- **데이터 관리 및 다른 애플리케이션의 데이터 제공**
- DB를 공유하기 위해 사용하며 애플리케이션 간 데이터 공유를 위해 표준화된 인터페이스 사용

  + SQLite DB/Web/파일 입출력을 통해 데이터 관리
  + 외부에서 실행 애플리케이션 내의 DB에 접근을 차단하면서 공유하고 싶은 데이터만 공개할 수 있게 도와줌
  + **용량이 큰 데이터**(음악, 사진 등)을 공유하는데 적합
  + 데이터 Read/Write 권한을 획득해야 애플리케이션 접근 가능
  + CRUD(Create, Read, Update, Delete) 원칙 준수

### Intent
- 컴포넌트 간 작업 수행을 위한 **정보 전달 통신수단**
- 다른 컴포넌트에 액션, 데이터 전달
- 인텐트를 통해 다른 애플리케이션의 컴포넌트 활성화 가능


## LifeCycle
- 사용자가 앱을 탐색하고, 나가고, 다시 돌아오는 등 행동에 따라 서로 다른 상태로 전환
- 이러한 활동을 알아차릴 수 있는 여러 콜백 제공
- 이를 제어함으로써 비정상 종료 및 상태가 저장되지 않는 문제를 해결 가능

### Activity
![activity](../image/android_activitylifecycle.png)

1. OnCreate()
- 시스템이 먼저 활동 생성 시 실행되는 것
- ViewModel과 연결 및 데이터 목록 바인딩

2. onStart()
- 액티비티가 사용자에게 표시되기 직전에 호출
- 앱이 UI를 관리하는 코드 초기화

3. onResume()
- 액티비티가 사용자와 상호작용할 때 호출
- 앱에서 포커스가 떠날 때까지 이 상태에 머무름

4. onPause()
- 활동이 포그라운드에 있지 않게 될 때 호출
- 배터리 수명에 영향을 미칠 수 있는 리소스 해제 가능
- 아주 잠깐 실행되므로 저장 작업에 시간이 부족

5. onStop()
- 활동이 사용자에게 더이상 표시되지 않으면 호출
- 화면에 보이지 않을 때 실행할 필요가 없는 기능 정지 가능
- CPU를 비교적 많이 소모하는 종료 작업 실행에 적합
- 여기서 화면이 다시 나타난다면 onRestart() 호출, 이후 onStart()부터 다시 호출

6. onDestroy()
- 활동이 소멸되기 전에 호출
- finish() 또는 구성 변경(기기 변경이나 멀티윈도우)로 인해 시스템이 일시적으로 활동이 소멸되는 경우 호출


#### onSaveInstanceState() / onRestoreInstanceState()
* 활동 정지 및 복구 시 상태 정보를 저장하는 메서드
* onSaveInstanceState()는 onPause() 이전에 호출
* onRestoreInstanceState()는 onStart() 직후에 호출

### Fragment
![fragment](../image/android_fragmentlifecycle.png)

1. onAttach()
- Activity에 붙을 때 처음 호출

2. onCreate()
- 초기화해야 하는 리소스를 초기화 및 Fragment 생성
- UI는 여기서 초기화되지 않는다.

3. onCreateView()
- Fragment의 View가 생성되는 부분
- Layout을 inflate하는 부분

4. onViewStateRestored()
- Activity의 onCreate() 이후 호출
- Activity 및 Fragment 뷰가 모두 생성된 상태로, view를 변경 가능하게 함

5. onStart()
- 유저에게 Fragment가 보이기 직전 호출
- Activity onStart() 한 이후 호출

6. onResume()
- 유저와 상호작용하기 직전에 호출

7. onPause()
- Focus를 잃은 상태에서 호출, background 상태

8. onStop()
- 화면이 완전히 가려질 시 호출
- 해당 액티비티를 재호출시 복원이 가능한 상태

9. onSaveInstanceState()
- 간단하고 가벼운 UI 상태를 저장
- API 28+에만 onStop() 이후 생성, 이전 API는 onStop() 이전에 생성

9. onDestroyView()
- Fragment 관련 View 제거 때 호출

10. onDestroy()
- 완전히 종료

11. onDetach()
- Activity로부터 Fragment 해제 때 호출

### Service


## Coroutine
### 코루틴이란?
+ **협력형 멀티태스킹**
+ 동시성 프로그래밍 지원
+ 비동기 처리를 쉽게 도와줌

### 간단한 설명
+ Co + routine
+ 보통 함수는 메인 루틴 및 서브 루틴으로 나뉘며 서브루틴은 1개의 입구 및 1개의 출구를 가짐
+ 코루틴 함수는 꼭 return 또는 닫는 괄호를 만나지 않더라도 **중간에 나갈 수 있음**.

### 동시성 프로그래밍
- 각자 다른 코드에 대해 조금씩 왔다갔다하면서 실행하는 개념
- 아예 똑같이 수행한는 병렬성 프로그래밍과 다른 개념

### Threading vs Coroutine
- 공통점: 동시성(Concurrency) 보장
- Threading
  + Stack 영역에 적재
  + OS Kernel을 통한 context switching을 통해 동시성 보장
  + Blocking을 통해 다른 작업이 끝날 때 까지 스레드가 사용하지 못하게 방지
- Coroutine
  + Heap 영역에 적재
  + 코딩을 통해 switching 시점을 정할 수 있음
  + suspend될 시 실제로 스레드가 완전 Block된것이 아니기 때문에 같은 스레드에 다른 작업을 하는 것이 가능
  + 다수 스레드가 동시에 수행된다면 context switching이 자주 일어나므로 단일 스레드에서 여러 coroutine object를 실행하는 것이 바랍직

### 문법
- GlobalScope: 프로그램 어디서나 제어 및 동작이 가능한 기능 범위
- CoroutineScope: 특정 목적의 Dispatcher를 지정, 제어 및 동작이 가능한 범위
  + Dispatcher.Default – 기본적인 백그라운드 동작
  + Dispatcher.IO – I/O 최적화
  + Dispatcher.Main – UI 스레드에서 동작
- launch: 반환값이 없는 Job 객체
- async: 반환값이 있는 deffered 객체
- runblocking: 코루틴 함수가 종료될 때까지 메인 루틴을 잠시 대기
- delay(millisecond: Long): 루틴 잠시 대기
- Job.join(): Job의 실행이 끝날때까지 대기
- Deffered.await(): defferd 실행이 끝날때까지 대기, 결과도 반환
- cancel(): delay() 또는 yield()함수가 사용된 위치까지 수행된 뒤 종료
suspend: suspend를 만나면 더 이상 아래 코드가 실행되지 않고 block 탈출


- 참고: https://aaronryu.github.io/2019/05/27/coroutine-and-thread/

## Room
- **앱으로부터 SQLite를 편리하게 사용할 수 있도록 하는 안드로이드 아키텍쳐 컴포넌트**
  + SQLite: 안드로이드에 기본 탑재된 내부 DB
  + 장점
    * 크기가 작음
    * 오픈 소스
    * **로컬**에서의 간단한 DB 구성
- 객체와 관계형 DB를 매핑(ORM)
- 구성 요소

![room](../image/android_room.png)

1. Entity: **DB 내의 테이블**, DB에 저장할 데이터 형식 정의
2. DAO(Data Access Object): DB에 접근하여 **수행할 작업**을 메소드 형태로 정의
3. Room Database: **DB의 전체적인 소유자** 역할, DB를 생성 및 버전 관리

- SQLite DB 라이브러리와 비교 장점
1. 컴파일 도중 SQL에 대한 유효성 검사 기능
2. Schema가 변경 시 자동 업데이트
3. 데이터 객체 변경을 위해 ORM 라이브러리 통해 매핑
4. LiveData 및 RxJava/Coroutine을 이용해 생성 가능

- 구현 코드
1. Entity
~~~java
@Entity(tableName = "Raid")
data class Raid (
        @PrimaryKey(autoGenerate = true) var raidId: Int,
        @ColumnInfo(name = "raidName") var raidName: String,
        @ColumnInfo(name = "raidThumbnail") var raidThumbnail: String,
        @ColumnInfo(name = "startDate") var startDate: Date,
        @ColumnInfo(name = "endDate") var endDate: Date,
        @Ignore var bosses : List<BossInRaid>
        ){
        constructor(raidName: String, raidThumbnail: String, startDate: Date, endDate: Date) :
                this(0, raidName, raidThumbnail, startDate, endDate, Collections.nCopies(4, BossInRaid()))
}
~~~
2. Dao
~~~java
@Dao
interface RaidDao {
  @Query("SELECT * FROM Raid")
    fun getAll(): List<Raid>

    @Query("SELECT * FROM Raid WHERE raidId IN (:raidIds)")
    fun loadAllByIds(raidIds: IntArray): List<Raid>
}
~~~
3. Room
~~~java
@Database(entities = [
    Raid::class], version = 1, exportSchema = false)
@TypeConverters(DataConverter::class)
abstract class AppDatabase : RoomDatabase() {
    abstract fun raidDao(): RaidDao

    companion object {
        private var INSTANCE: AppDatabase? = null

        fun getInstance(context: Context): AppDatabase? {
            if (INSTANCE == null) {
                synchronized(AppDatabase::class) {
                    INSTANCE = Room.databaseBuilder(context.applicationContext,
                            AppDatabase::class.java, "database")
                        .createFromAsset("database/database.db")
                        .allowMainThreadQueries()
                        .build()
                }
            }
            return INSTANCE
        }
    }
}
~~~
