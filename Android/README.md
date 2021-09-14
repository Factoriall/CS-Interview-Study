# Android

- [Manifest 파일](#Manifest-파일)
- [4대 컴포넌트](#4대-컴포넌트)
- [LifeCycle](#LifeCycle)
  + Activity
  + Fragment
  + Service
- [Coroutine](Coroutine)


## Manifest 파일

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
![components](../image/android_activitylifecycle.png)

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
* onRestoreInstanceState()는 onStart() 직후에 호출

### Fragment

### Service


## Coroutine
