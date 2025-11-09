# 온보딩 가이드 - Android JL OTA SDK

환영합니다! 이 문서는 Android JL OTA SDK 프로젝트에 새로 합류한 개발자들을 위한 온보딩 가이드입니다.

## 목차

1. [프로젝트 소개](#프로젝트-소개)
2. [시작하기](#시작하기)
3. [개발 환경 설정](#개발-환경-설정)
4. [프로젝트 구조](#프로젝트-구조)
5. [핵심 개념](#핵심-개념)
6. [개발 워크플로우](#개발-워크플로우)
7. [테스트](#테스트)
8. [문제 해결](#문제-해결)
9. [추가 리소스](#추가-리소스)

---

## 프로젝트 소개

**Android JL OTA SDK**는 JieLi(杰理) 디바이스를 위한 Bluetooth OTA(Over-The-Air) 펌웨어 업데이트 솔루션입니다.

### 주요 기능

- ✅ **BLE(Bluetooth Low Energy)** 및 **SPP(Serial Port Profile)** 통신 지원
- ✅ TWS 이어폰 및 단일 디바이스 OTA 지원
- ✅ 다중 디바이스 동시 업그레이드 지원
- ✅ Android 14+ 호환
- ✅ x86, x86_64, ARM 플랫폼 지원

### 버전 정보

- **최신 버전**: APP_V1.8.1_SDK_V1.10.0
- **릴리스 날짜**: 2025년 8월 11일
- **담당자**: 钟卓成

---

## 시작하기

### 사전 요구사항

개발을 시작하기 전에 다음 항목들이 설치되어 있어야 합니다:

#### 필수 항목

- **Android Studio**: Arctic Fox (2020.3.1) 이상
- **JDK**: 11 이상
- **Gradle**: 7.0 이상
- **Android SDK**:
  - Minimum SDK: API 21 (Android 5.0)
  - Target SDK: API 34 (Android 14)
  - Compile SDK: API 34

#### 권장 항목

- Git 2.30 이상
- Android 디바이스 (실제 Bluetooth 테스트용)
- JieLi 디바이스 (OTA 테스트용)

### 저장소 클론

```bash
git clone https://github.com/Jieli-Tech/Android-JL_OTA.git
cd Android-JL_OTA
```

---

## 개발 환경 설정

### 1. Android Studio에서 프로젝트 열기

1. Android Studio 실행
2. **File → Open** 선택
3. `Android-JL_OTA/code/JL_OTA_Android_V1.8.1_SDK_V1.10.0` 디렉토리 선택
4. Gradle 동기화 완료 대기

### 2. SDK 라이브러리 설정 확인

프로젝트는 다음 SDK 라이브러리를 사용합니다:

```
libs/
├── jl_bt_ota_V1.10.0_10931-release.aar
└── jl_bt_ota_V1.10.0_10932-release.aar
```

### 3. 권한 설정

앱이 정상 작동하려면 다음 권한들이 필요합니다:

- `BLUETOOTH` / `BLUETOOTH_ADMIN` (Android 11 이하)
- `BLUETOOTH_SCAN` / `BLUETOOTH_CONNECT` (Android 12+)
- `ACCESS_FINE_LOCATION` (Bluetooth 스캔용)
- `READ_EXTERNAL_STORAGE` / `WRITE_EXTERNAL_STORAGE`
- `MANAGE_EXTERNAL_STORAGE` (Android 11+)

### 4. 첫 빌드 실행

```bash
cd code/JL_OTA_Android_V1.8.1_SDK_V1.10.0
./gradlew clean build
```

또는 Android Studio에서:
- **Build → Make Project** (Ctrl+F9 / Cmd+F9)

---

## 프로젝트 구조

```
Android-JL_OTA/
├── apk/                          # 릴리스 APK 파일
│   └── JLOTA_V1.8.1_10807-debug.apk
├── code/                         # 소스 코드
│   └── JL_OTA_Android_V1.8.1_SDK_V1.10.0/
│       ├── otasdk/              # 메인 앱 모듈
│       │   ├── src/
│       │   │   └── main/
│       │   │       ├── java/com/jieli/otasdk/
│       │   │       │   ├── activities/     # UI 액티비티
│       │   │       │   ├── fragments/      # UI 프래그먼트
│       │   │       │   ├── tool/           # 핵심 OTA 로직
│       │   │       │   ├── viewmodel/      # MVVM 뷰모델
│       │   │       │   ├── adapter/        # 리사이클러뷰 어댑터
│       │   │       │   └── util/           # 유틸리티 클래스
│       │   │       ├── res/                # 리소스 (레이아웃, 문자열 등)
│       │   │       └── AndroidManifest.xml
│       │   └── build.gradle
│       ├── build.gradle
│       └── settings.gradle
├── doc/                          # 문서
│   ├── JieLi_OTA_SDK_Android_Development_Doc.url
│   └── 杰理OTA外接库(Android)开发文档.url
├── libs/                         # SDK 라이브러리
│   ├── jl_bt_ota_V1.10.0_10931-release.aar
│   └── jl_bt_ota_V1.10.0_10932-release.aar
├── README.md                     # 프로젝트 개요
└── LICENSE                       # 라이선스 정보
```

### 핵심 디렉토리 설명

| 디렉토리 | 설명 |
|---------|------|
| `otasdk/tool/ota/` | OTA 업데이트 핵심 로직 (OTAManager, 통신 처리) |
| `otasdk/viewmodel/` | MVVM 아키텍처의 뷰모델 |
| `otasdk/activities/` | 메인 UI 화면들 |
| `otasdk/util/` | 헬퍼 함수 및 유틸리티 |

---

## 핵심 개념

### 1. OTA (Over-The-Air) 업데이트

OTA는 무선으로 디바이스 펌웨어를 업데이트하는 기술입니다.

**지원 프로토콜:**
- **BLE (Bluetooth Low Energy)**: 기본값, 저전력 블루투스 통신
- **SPP (Serial Port Profile)**: 클래식 블루투스 시리얼 통신

### 2. OTA 워크플로우

```
1. 디바이스 검색 및 연결
   ↓
2. 펌웨어 파일 선택
   ↓
3. OTA 매니저 설정
   ↓
4. 펌웨어 전송
   ↓
5. 디바이스 재부팅 및 업데이트 적용
   ↓
6. 업데이트 완료 확인
```

### 3. OTA 설정 옵션

`BluetoothOTAConfigure` 클래스를 통해 설정:

```kotlin
val bluetoothOption = BluetoothOTAConfigure()

// 통신 방식 선택 (BLE 우선)
bluetoothOption.priority = BluetoothOTAConfigure.PREFER_BLE

// 사용자 정의 재연결 방식 사용 여부
bluetoothOption.isUseReconnect = !JL_Constant.NEED_CUSTOM_RECONNECT_WAY

// 디바이스 인증 활성화 (펌웨어 엔지니어 확인 필요)
bluetoothOption.isUseAuthDevice = JL_Constant.IS_NEED_DEVICE_AUTH

// MTU 설정 (Deprecated)
bluetoothOption.mtu = BluetoothConstant.BLE_MTU_MIN
bluetoothOption.isNeedChangeMtu = false

// JieLi 서버 사용 (현재 미지원)
bluetoothOption.isUseJLServer = false

// OTAManager에 설정 적용
OTAManager.getInstance().configure(bluetoothOption)
```

### 4. 로깅 시스템

```java
// 로그 출력 설정
JL_Log.setIsLog(BuildConfig.DEBUG)

// 로그 파일 저장 설정
JL_Log.setIsSaveLogFile(context, BuildConfig.DEBUG)
```

**로그 파일 위치**: `Android/data/[패키지명]/files/logcat/`

**로그 파일 형식**: `ota_log_app_[timestamp].txt`

---

## 개발 워크플로우

### 1. 새 기능 개발

#### Step 1: 브랜치 생성
```bash
git checkout -b feature/your-feature-name
```

#### Step 2: 코드 작성
- MVVM 패턴 준수
- Kotlin 코딩 컨벤션 따르기
- 주석 추가 (중요한 로직에 대해)

#### Step 3: 테스트
```bash
./gradlew test
./gradlew connectedAndroidTest
```

#### Step 4: 커밋 및 푸시
```bash
git add .
git commit -m "feat: Add your feature description"
git push origin feature/your-feature-name
```

### 2. 버그 수정

#### Step 1: 이슈 확인
- GitHub Issues에서 버그 리포트 확인
- 재현 단계 파악

#### Step 2: 로그 분석
- `Android/data/com.jieli.otasdk/files/logcat/` 에서 로그 확인
- 에러 스택 트레이스 분석

#### Step 3: 수정 및 검증
- 버그 수정
- 재현 테스트
- 회귀 테스트

### 3. 코드 리뷰 가이드라인

Pull Request 작성 시 포함할 내용:
- 변경 사항 요약
- 테스트 결과
- 스크린샷 (UI 변경 시)
- Breaking Changes 여부

---

## 테스트

### 1. 단위 테스트

```bash
./gradlew test
```

### 2. 통합 테스트

```bash
./gradlew connectedAndroidTest
```

### 3. 수동 테스트

#### OTA 업데이트 테스트 절차

1. **테스트 APK 설치**
   ```bash
   adb install apk/JLOTA_V1.8.1_10807-debug.apk
   ```

2. **펌웨어 파일 준비**
   - 펌웨어 파일을 다음 경로에 복사:
   ```
   /Android/data/com.jieli.otasdk/files/upgrade/
   ```

3. **디바이스 연결**
   - 앱에서 Bluetooth 활성화
   - 대상 JieLi 디바이스 검색 및 연결

4. **OTA 실행**
   - 펌웨어 파일 선택
   - OTA 시작
   - 진행 상황 모니터링

5. **결과 확인**
   - 업데이트 성공 여부 확인
   - 로그 파일 검토

### 4. 테스트 체크리스트

- [ ] BLE 연결 테스트
- [ ] SPP 연결 테스트
- [ ] 단일 디바이스 OTA
- [ ] TWS 이어폰 OTA
- [ ] 다중 디바이스 OTA
- [ ] 권한 처리 (Android 14)
- [ ] 재연결 메커니즘
- [ ] 에러 핸들링

---

## 문제 해결

### 자주 발생하는 문제

#### 1. Bluetooth 연결 실패

**증상**: 디바이스 검색은 되지만 연결이 안 됨

**해결 방법**:
- Android 버전별 권한 확인
  - Android 12+: `BLUETOOTH_SCAN`, `BLUETOOTH_CONNECT` 필요
  - Android 6-11: `ACCESS_FINE_LOCATION` 필요
- 위치 서비스(GPS) 활성화 확인
- 앱 재시작

#### 2. OTA 전송 중 실패

**증상**: 업데이트 중간에 실패

**해결 방법**:
- MTU 설정 확인
- 디바이스와 거리 확인 (1m 이내 권장)
- 배터리 잔량 확인 (디바이스)
- 로그 파일 분석

#### 3. Android 14 저장소 권한 문제

**증상**: 펌웨어 파일 접근 불가

**해결 방법**:
- `MANAGE_EXTERNAL_STORAGE` 권한 요청
- Scoped Storage API 사용 확인
- 앱 특정 디렉토리 사용

#### 4. BLE 전송 속도 저하

**증상**: 데이터 전송이 비정상적으로 느림

**해결 방법**:
- SDK V1.9.3 이상 버전 사용 (수정됨)
- Connection Interval 확인
- 로그에서 패킷 전송 상태 확인

### 로그 분석 방법

1. **로그 파일 위치**:
   ```
   /Android/data/com.jieli.otasdk/files/logcat/ota_log_app_[timestamp].txt
   ```

2. **주요 로그 키워드**:
   - `OTA_START`: OTA 시작
   - `OTA_PROGRESS`: 진행률
   - `OTA_SUCCESS`: 성공
   - `OTA_FAILED`: 실패
   - `ERROR`: 에러 발생

3. **로그 추출**:
   ```bash
   adb pull /sdcard/Android/data/com.jieli.otasdk/files/logcat/ ./logs/
   ```

### 이슈 리포팅

문제가 해결되지 않을 경우:

1. **간단한 문제 현상 설명** (필수)
2. **최신 로그 파일 제공** (필수)
3. 발생 시간대 명시
4. 스크린샷 또는 영상 첨부

GitHub Issues: https://github.com/Jieli-Tech/Android-JL_OTA/issues

---

## 추가 리소스

### 공식 문서

- [JieLi OTA SDK 개발 문서 (중국어)](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/index.html)
- [FAQ (자주 묻는 질문)](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/other/qa.html)

### 참고 자료

- **Android Bluetooth 가이드**: https://developer.android.com/guide/topics/connectivity/bluetooth
- **Android 권한 가이드**: https://developer.android.com/guide/topics/permissions/overview
- **MVVM 아키텍처**: https://developer.android.com/topic/architecture

### 커뮤니티

- GitHub Issues: 기술 질문 및 버그 리포트
- 공식 문서 Q&A 섹션

---

## 빠른 참조

### 주요 명령어

```bash
# 프로젝트 클론
git clone https://github.com/Jieli-Tech/Android-JL_OTA.git

# 빌드
./gradlew clean build

# 디버그 APK 설치
adb install -r apk/JLOTA_V1.8.1_10807-debug.apk

# 로그 확인
adb logcat | grep "JL_OTA"

# 로그 파일 추출
adb pull /sdcard/Android/data/com.jieli.otasdk/files/logcat/
```

### 주요 파일 경로

| 설명 | 경로 |
|------|------|
| OTA 매니저 | `otasdk/src/main/java/com/jieli/otasdk/tool/ota/OTAManager` |
| Bluetooth 설정 | `otasdk/src/main/java/com/jieli/otasdk/tool/ota/BluetoothOTAConfigure` |
| 메인 액티비티 | `otasdk/src/main/java/com/jieli/otasdk/activities/` |
| 펌웨어 저장 위치 | `/Android/data/com.jieli.otasdk/files/upgrade/` |
| 로그 파일 위치 | `/Android/data/com.jieli.otasdk/files/logcat/` |

---

## 다음 단계

온보딩이 완료되었나요? 이제 다음을 시도해보세요:

1. ✅ 데모 앱 빌드 및 실행
2. ✅ 실제 디바이스로 OTA 테스트
3. ✅ 코드베이스 탐색 (특히 `tool/ota/` 디렉토리)
4. ✅ 첫 이슈 선택 및 해결
5. ✅ 첫 Pull Request 제출

**궁금한 점이 있으면 언제든지 Issues를 통해 질문해주세요!**

---

마지막 업데이트: 2025-11-09
