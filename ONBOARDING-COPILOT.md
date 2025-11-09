# 온보딩 가이드 (Onboarding Guide)

Android-JL_OTA 프로젝트에 오신 것을 환영합니다! 이 문서는 프로젝트에 빠르게 익숙해지고 개발을 시작할 수 있도록 도와드립니다.

## 📋 목차

1. [프로젝트 소개](#프로젝트-소개)
2. [시스템 요구사항](#시스템-요구사항)
3. [개발 환경 설정](#개발-환경-설정)
4. [프로젝트 구조](#프로젝트-구조)
5. [빠른 시작 가이드](#빠른-시작-가이드)
6. [주요 개념 및 용어](#주요-개념-및-용어)
7. [SDK 통합 가이드](#sdk-통합-가이드)
8. [테스트 및 디버깅](#테스트-및-디버깅)
9. [문제 해결](#문제-해결)
10. [다음 단계](#다음-단계)

---

## 프로젝트 소개

**Android-JL_OTA**는 안드로이드용 블루투스 OTA(Over-The-Air) 업데이트 SDK입니다. 이 프로젝트는 개발자들이 JieLi(杰理) 블루투스 장치에 대한 펌웨어 업데이트 기능을 안드로이드 애플리케이션에 쉽게 통합할 수 있도록 도와줍니다.

### 주요 기능
- ✅ BLE 및 SPP 블루투스 통신 지원
- ✅ 펌웨어 OTA 업데이트
- ✅ 여러 장치 동시 업그레이드 지원
- ✅ Android 14+ 완벽 지원
- ✅ 디바이스 인증 및 보안
- ✅ 상세한 로그 및 디버깅 도구

### 지원 플랫폼
- Android 5.0 (API 21) 이상
- ARM, ARM64, x86, x86_64 아키텍처

---

## 시스템 요구사항

### 개발 환경
- **Android Studio**: Arctic Fox (2020.3.1) 이상 권장
- **JDK**: Java 11 이상
- **Gradle**: 8.0+ (프로젝트에 포함됨)
- **Kotlin**: 2.2.0 (프로젝트에 포함됨)
- **최소 Android SDK**: API 21 (Android 5.0)
- **타겟 Android SDK**: API 35 (Android 14+)

### 테스트 장치
- 블루투스를 지원하는 안드로이드 기기
- JieLi 칩셋이 탑재된 블루투스 장치 (이어폰, 스피커 등)

---

## 개발 환경 설정

### 1. 저장소 클론

```bash
git clone https://github.com/crazyupinc/Android-JL_OTA.git
cd Android-JL_OTA
```

### 2. Android Studio에서 프로젝트 열기

1. Android Studio를 실행합니다
2. `File` → `Open` 선택
3. 클론한 프로젝트의 `code/JL_OTA_Android_V1.8.1_SDK_V1.10.0/` 디렉토리를 선택
4. Gradle Sync가 자동으로 시작됩니다

### 3. SDK 및 의존성 설치

프로젝트를 처음 열면 Android Studio가 필요한 SDK와 의존성을 자동으로 다운로드합니다. 다음 항목들이 설치되었는지 확인하세요:

- Android SDK Platform
- Android SDK Build-Tools
- Kotlin 플러그인

### 4. 권한 설정

안드로이드 매니페스트에 다음 권한들이 포함되어 있는지 확인하세요:

- `BLUETOOTH`
- `BLUETOOTH_ADMIN`
- `BLUETOOTH_SCAN`
- `BLUETOOTH_CONNECT`
- `ACCESS_FINE_LOCATION`
- `READ_EXTERNAL_STORAGE` / `WRITE_EXTERNAL_STORAGE` (Android 10 미만)

---

## 프로젝트 구조

```
Android-JL_OTA/
├── apk/                              # 테스트용 APK 파일
│   └── JLOTA_V1.8.1_10807-debug.apk
├── code/                             # 소스 코드
│   └── JL_OTA_Android_V1.8.1_SDK_V1.10.0/
│       ├── otasdk/                   # 메인 애플리케이션 모듈
│       │   ├── src/                  # 소스 코드
│       │   ├── libs/                 # 로컬 라이브러리
│       │   └── docs/                 # 문서
│       ├── build.gradle              # 프로젝트 빌드 설정
│       └── settings.gradle           # 프로젝트 설정
├── doc/                              # 개발 문서 링크
├── libs/                             # 핵심 OTA SDK 라이브러리 (AAR)
│   ├── jl_bt_ota_V1.10.0_10931-release.aar
│   └── jl_bt_ota_V1.10.0_10932-release.aar
├── LICENSE
└── README.md                         # 프로젝트 README
```

### 주요 모듈 설명

#### `otasdk/` - 데모 애플리케이션
- JL OTA SDK를 사용하는 참조 구현
- UI 샘플 및 통합 예제 포함
- 패키지 구조:
  - `com.jieli.otasdk/tool/ota/` - SDK 통합 예제

#### `libs/` - 핵심 SDK
- `jl_bt_ota_V1.10.0_*.aar` - OTA 기능을 제공하는 핵심 라이브러리

---

## 빠른 시작 가이드

### 1단계: 데모 앱 빌드 및 실행

```bash
# 프로젝트 디렉토리로 이동
cd code/JL_OTA_Android_V1.8.1_SDK_V1.10.0

# Gradle로 빌드
./gradlew assembleDebug

# 또는 Android Studio에서 Run 버튼 클릭
```

### 2단계: 테스트용 APK 설치

미리 빌드된 APK를 사용하려면:

```bash
adb install apk/JLOTA_V1.8.1_10807-debug.apk
```

### 3단계: 펌웨어 파일 준비

업그레이드 파일을 다음 경로에 복사하세요:

```
/Android/data/com.jieli.otasdk/files/upgrade/
```

**참고**: 
- Android 10 이상: `/Android/data/com.jieli.otasdk/files/upgrade/`
- Android 10 미만: `/com.jieli.otasdk/upgrade/`

### 4단계: 장치 연결 및 OTA 실행

1. 앱을 실행하고 필요한 권한을 승인합니다
2. 업그레이드할 블루투스 장치에 연결합니다
3. 펌웨어 파일을 선택합니다
4. OTA 업그레이드를 시작합니다

---

## 주요 개념 및 용어

### OTA (Over-The-Air)
무선으로 장치의 펌웨어를 업데이트하는 기술입니다. 사용자가 장치를 물리적으로 연결하지 않고도 소프트웨어를 업데이트할 수 있습니다.

### BLE vs SPP
- **BLE (Bluetooth Low Energy)**: 저전력 블루투스 통신. 기본 통신 방식
- **SPP (Serial Port Profile)**: 클래식 블루투스 시리얼 통신. 펌웨어 지원 필요

### MTU (Maximum Transmission Unit)
BLE 통신에서 한 번에 전송할 수 있는 최대 데이터 크기입니다.

### 디바이스 인증
OTA 업데이트 전에 장치의 정당성을 확인하는 보안 프로세스입니다. 공식 펌웨어는 기본적으로 활성화되어 있습니다.

### 회선 재연결 (Reconnect)
OTA 업데이트 중 장치가 재부팅될 때 자동으로 재연결하는 메커니즘입니다.

---

## SDK 통합 가이드

### 프로젝트에 SDK 추가하기

#### 1. AAR 파일 추가

`build.gradle (Module)`에 다음을 추가:

```gradle
dependencies {
    implementation files('libs/jl_bt_ota_V1.10.0_10931-release.aar')
    // 또는
    implementation files('libs/jl_bt_ota_V1.10.0_10932-release.aar')
}
```

#### 2. OTA Manager 초기화

```kotlin
import com.jieli.otasdk.tool.ota.OTAManager
import com.jieli.otasdk.tool.ota.model.BluetoothOTAConfigure

// Application 또는 초기화 코드에서
val bluetoothOption = BluetoothOTAConfigure()

// 통신 방식 선택 (BLE 또는 SPP)
bluetoothOption.priority = BluetoothOTAConfigure.PREFER_BLE

// 사용자 정의 재연결 사용 여부
bluetoothOption.isUseReconnect = true

// 디바이스 인증 활성화 (펌웨어 엔지니어와 확인)
bluetoothOption.isUseAuthDevice = true

// OTA 설정 적용
OTAManager.getInstance().configure(bluetoothOption)
```

#### 3. 로그 설정

```kotlin
import com.jieli.jl_bt_ota.util.JL_Log

// 개발 환경
JL_Log.setIsLog(true)  // 로그 출력 활성화
JL_Log.setIsSaveLogFile(context, true)  // 로그 파일 저장

// 프로덕션 환경
JL_Log.setIsLog(false)
JL_Log.setIsSaveLogFile(context, false)
```

### 기본 OTA 작업 흐름

```kotlin
// 1. 장치 연결
// (블루투스 연결 로직 구현)

// 2. OTA 준비
val otaManager = OTAManager.getInstance()

// 3. 펌웨어 파일 선택
val firmwareFile = File("/path/to/firmware.ufw")

// 4. OTA 시작
otaManager.startOTA(device, firmwareFile, object : OTACallback {
    override fun onProgress(progress: Int) {
        // 진행률 업데이트
    }
    
    override fun onSuccess() {
        // OTA 성공
    }
    
    override fun onError(error: Int, message: String) {
        // 에러 처리
    }
})
```

---

## 테스트 및 디버깅

### 로그 파일 위치

로그 파일은 다음 위치에 저장됩니다:

```
/Android/data/com.jieli.otasdk/files/logcat/
```

파일 형식: `ota_log_app_[timestamp].txt`

예시: `ota_log_app_20220330093020.432.txt`

### 디버그 모드 활성화

`build.gradle`에서 디버그 모드 설정:

```gradle
android {
    buildTypes {
        debug {
            debuggable true
            minifyEnabled false
        }
    }
}
```

### 일반적인 테스트 시나리오

1. **정상 OTA 업데이트**
   - 장치 연결 → 펌웨어 선택 → 업데이트 시작 → 진행률 모니터링 → 완료 확인

2. **연결 끊김 테스트**
   - OTA 중 장치의 전원을 끄거나 범위 밖으로 이동
   - 재연결 메커니즘 동작 확인

3. **잘못된 펌웨어 테스트**
   - 호환되지 않는 펌웨어 파일로 시도
   - 에러 처리 확인

4. **멀티 디바이스 테스트**
   - 여러 장치 동시 업데이트
   - 각 장치의 독립적인 진행 확인

---

## 문제 해결

### 자주 묻는 질문 (FAQ)

#### Q1: OTA 업데이트가 시작되지 않아요
**A**: 다음 사항을 확인하세요:
- 블루투스 권한이 승인되었는지 확인
- 장치가 제대로 연결되었는지 확인
- 펌웨어 파일 경로가 올바른지 확인
- 로그 파일에서 에러 메시지 확인

#### Q2: Android 14에서 권한 에러가 발생해요
**A**: Android 14+에서는 다음 권한이 필요합니다:
- `BLUETOOTH_SCAN`
- `BLUETOOTH_CONNECT`
- `BLUETOOTH_ADVERTISE`

런타임에서 이러한 권한을 요청해야 합니다.

#### Q3: OTA 중에 연결이 끊어졌어요
**A**: 
- 자동 재연결이 활성화되어 있는지 확인 (`isUseReconnect = true`)
- 장치가 블루투스 범위 내에 있는지 확인
- 배터리가 충분한지 확인

#### Q4: 로그 파일을 찾을 수 없어요
**A**: 
- Android 10 이상에서는 스토리지 권한이 필요합니다
- `JL_Log.setIsSaveLogFile(context, true)`가 호출되었는지 확인
- 파일 탐색기에서 앱 전용 디렉토리 접근 권한 확인

### 에러 코드 참조

로그에서 에러 코드를 확인한 후, 다음 리소스를 참조하세요:
- [공식 문서](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/index.html)
- [FAQ 페이지](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/other/qa.html)

### 버그 리포트 제출

문제가 해결되지 않으면 GitHub Issue를 생성해주세요. 다음 정보를 포함해야 합니다:

1. **문제 설명** (필수)
   - 무엇을 하려고 했는가?
   - 무엇이 잘못되었는가?

2. **로그 파일** (필수)
   - 문제 발생 시간과 가장 가까운 타임스탬프의 로그 파일

3. **환경 정보**
   - Android 버전
   - 장치 모델
   - SDK 버전

4. **재현 단계**
   - 문제를 재현할 수 있는 단계별 설명

5. **스크린샷/비디오** (선택사항)

---

## 다음 단계

축하합니다! 이제 Android-JL_OTA 프로젝트의 기본을 이해하셨습니다. 다음 단계를 진행해보세요:

### 🚀 추가 학습 리소스

1. **공식 문서 읽기**
   - [한국어 개발 문서](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/index.html)
   - [영어 개발 문서](https://doc.zh-jieli.com/Apps/Android/ota/en-us/master/index.html)

2. **데모 앱 코드 탐색**
   - `code/JL_OTA_Android_V1.8.1_SDK_V1.10.0/otasdk/`
   - 다양한 사용 사례와 구현 패턴 학습

3. **고급 기능 학습**
   - 사용자 정의 재연결 메커니즘
   - MTU 최적화
   - 멀티 디바이스 관리
   - 네트워크 OTA (향후 지원 예정)

### 📚 참조 자료

- **GitHub 저장소**: [Android-JL_OTA](https://github.com/crazyupinc/Android-JL_OTA)
- **공식 문서**: [JieLi OTA SDK Documentation](https://doc.zh-jieli.com/Apps/Android/ota/)
- **API 레퍼런스**: SDK AAR 파일의 Javadoc/KDoc 참조
- **업데이트 로그**: [README.md](./README.md#更新日志)

### 💡 커뮤니티 및 지원

- **Issue Tracker**: GitHub Issues에서 버그 리포트 및 기능 요청
- **FAQ**: [자주 묻는 질문](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/other/qa.html)
- **프로젝트 담당자**: 
  - 현재 버전 책임자: 钟卓成

### 🎯 권장 개발 경로

1. **초급 개발자**
   - 데모 앱 실행 및 테스트
   - 기본 OTA 작업 흐름 이해
   - 간단한 UI 커스터마이징

2. **중급 개발자**
   - 자신의 앱에 SDK 통합
   - 커스텀 재연결 로직 구현
   - 에러 처리 및 예외 상황 관리

3. **고급 개발자**
   - 멀티 디바이스 동시 업데이트
   - 성능 최적화
   - 보안 강화 및 디바이스 인증
   - SDK 확장 및 고급 기능 개발

---

## 🤝 기여하기

프로젝트 개선에 관심이 있으시다면:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 라이선스

이 프로젝트는 LICENSE 파일에 명시된 라이선스를 따릅니다.

---

## 📞 연락처

문제가 있거나 질문이 있으시면:
- GitHub Issues 생성
- 공식 문서의 FAQ 참조
- 프로젝트 메인테이너에게 연락

**즐거운 코딩 되세요! 🎉**
