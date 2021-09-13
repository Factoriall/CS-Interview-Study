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
- Background에서 특정 작업을 처리하기 위해 사용하는 컴포넌트
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

## Coroutine
