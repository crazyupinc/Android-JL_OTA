# Android JL OTA SDK 기여 가이드

Android JL OTA SDK 프로젝트에 기여하는 것을 고려해주셔서 감사합니다! 이 문서는 기여를 위한 가이드라인과 절차를 제공합니다.

## 목차

1. [행동 강령](#행동-강령)
2. [시작하기](#시작하기)
3. [개발 프로세스](#개발-프로세스)
4. [코딩 표준](#코딩-표준)
5. [커밋 가이드라인](#커밋-가이드라인)
6. [Pull Request 절차](#pull-request-절차)
7. [테스트 가이드라인](#테스트-가이드라인)
8. [문서화](#문서화)

---

## 행동 강령

### 우리의 약속

우리는 모든 기여자에게 환영받고 포용적인 환경을 제공하기 위해 노력합니다.

### 기대되는 행동

- 존중하고 배려하는 태도
- 건설적인 비판을 우아하게 수용
- 프로젝트와 커뮤니티에 최선인 것에 집중
- 다른 커뮤니티 구성원에게 공감 표시

### 용납되지 않는 행동

- 괴롭힘이나 차별적 언어
- 트롤링이나 모욕적인 댓글
- 공개 또는 사적인 괴롭힘
- 허락 없이 타인의 개인 정보 게시

---

## 시작하기

### 사전 요구사항

기여하기 전에 다음을 확인하세요:

1. [ONBOARDING_KR.md](ONBOARDING_KR.md) 가이드 읽기
2. 개발 환경 설정
3. 프로젝트 빌드 및 실행 성공
4. 코드베이스 숙지

### 작업할 이슈 찾기

1. [Issues](https://github.com/Jieli-Tech/Android-JL_OTA/issues) 페이지 확인
2. 다음 라벨이 붙은 이슈 찾기:
   - `good first issue` - 초보자에게 적합
   - `help wanted` - 커뮤니티 기여 환영
   - `bug` - 버그 수정 필요
   - `enhancement` - 새 기능 또는 개선

### 이슈 선점하기

1. 이슈에 관심을 표현하는 댓글 작성
2. 메인테이너 승인 대기
3. 확인 후 작업 시작

---

## 개발 프로세스

### 1. Fork 및 Clone

```bash
# GitHub에서 저장소를 Fork한 후:
git clone https://github.com/YOUR_USERNAME/Android-JL_OTA.git
cd Android-JL_OTA

# upstream 원격 저장소 추가
git remote add upstream https://github.com/Jieli-Tech/Android-JL_OTA.git
```

### 2. 브랜치 생성

```bash
# 로컬 main 브랜치 업데이트
git checkout main
git pull upstream main

# 새 브랜치 생성
git checkout -b feature/your-feature-name
# 또는
git checkout -b fix/issue-description
```

브랜치 명명 규칙:
- `feature/` - 새로운 기능
- `fix/` - 버그 수정
- `docs/` - 문서 업데이트
- `refactor/` - 코드 리팩토링
- `test/` - 테스트 추가 또는 수정
- `chore/` - 유지보수 작업

### 3. 변경사항 작성

- 깨끗하고 유지보수 가능한 코드 작성
- 기존 코드 스타일 따르기
- 복잡한 로직에 주석 추가
- 필요시 문서 업데이트

### 4. 변경사항 테스트

```bash
# 단위 테스트 실행
./gradlew test

# 계측 테스트 실행
./gradlew connectedAndroidTest

# 프로젝트 빌드
./gradlew clean build
```

### 5. 변경사항 커밋

아래 [커밋 가이드라인](#커밋-가이드라인) 참조.

### 6. Fork로 푸시

```bash
git push origin feature/your-feature-name
```

### 7. Pull Request 생성

아래 [Pull Request 절차](#pull-request-절차) 참조.

---

## 코딩 표준

### Kotlin 스타일 가이드

[Kotlin 코딩 컨벤션](https://kotlinlang.org/docs/coding-conventions.html) 준수:

#### 네이밍

```kotlin
// 클래스: PascalCase
class OTAManager { }

// 함수: camelCase
fun startOtaUpdate() { }

// 프로퍼티: camelCase
val deviceName: String

// 상수: UPPER_SNAKE_CASE
const val MAX_RETRY_COUNT = 3

// Private 프로퍼티: 언더스코어로 시작 (선택사항)
private val _connectionState = MutableLiveData<ConnectionState>()
```

#### 코드 구성

```kotlin
class ExampleClass {
    // 1. Companion object
    companion object {
        const val TAG = "ExampleClass"
    }

    // 2. 프로퍼티
    private var state: State = State.IDLE

    // 3. Init 블록
    init {
        // 초기화
    }

    // 4. Public 함수
    fun publicMethod() { }

    // 5. Private 함수
    private fun privateMethod() { }

    // 6. Inner 클래스
    inner class InnerClass { }
}
```

#### 코드 포맷팅

- **들여쓰기**: 4개 스페이스 (탭 사용 금지)
- **줄 길이**: 최대 120자
- **빈 줄**: 논리적 섹션 구분에 사용
- **중괄호**: K&R 스타일 (같은 줄에 여는 중괄호)

```kotlin
// 좋은 예
if (condition) {
    doSomething()
} else {
    doSomethingElse()
}

// 나쁜 예
if (condition)
{
    doSomething()
}
```

### Java 스타일 가이드

기존 Java 코드는 [Google Java 스타일 가이드](https://google.github.io/styleguide/javaguide.html) 준수:

```java
// 클래스 선언
public class OTAManager {
    // 상수
    private static final String TAG = "OTAManager";

    // 멤버 변수
    private int retryCount;

    // 생성자
    public OTAManager() {
        this.retryCount = 0;
    }

    // 메서드
    public void startUpdate() {
        // 구현
    }
}
```

### XML 리소스

#### 레이아웃 파일

```xml
<!-- 의미있는 ID 사용 -->
<TextView
    android:id="@+id/tv_device_name"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/device_name" />
```

#### 문자열 리소스

```xml
<!-- 항상 문자열 리소스 사용, 하드코딩 금지 -->
<string name="ota_update_success">OTA 업데이트가 성공적으로 완료되었습니다</string>
<string name="ota_update_failed">OTA 업데이트 실패: %1$s</string>
```

### 주석

```kotlin
/**
 * 연결된 디바이스의 OTA 업데이트 프로세스를 시작합니다.
 *
 * @param firmwareFile 업로드할 펌웨어 파일
 * @param callback 업데이트 진행 상황 및 완료 콜백
 * @throws IllegalStateException 디바이스가 연결되지 않은 경우
 */
fun startOtaUpdate(
    firmwareFile: File,
    callback: OTACallback
) {
    // 디바이스 연결 확인
    if (!isDeviceConnected()) {
        throw IllegalStateException("디바이스가 연결되지 않았습니다")
    }

    // TODO: 재시도 메커니즘 추가
    // FIXME: 파일이 손상된 경우의 엣지 케이스 처리

    // 전송 시작
    transferFirmware(firmwareFile, callback)
}
```

---

## 커밋 가이드라인

### 커밋 메시지 형식

[Conventional Commits](https://www.conventionalcommits.org/) 사양 준수:

```
<type>(<scope>): <subject>

<body>

<footer>
```

#### Type

- `feat`: 새로운 기능
- `fix`: 버그 수정
- `docs`: 문서 변경
- `style`: 코드 스타일 변경 (포맷팅, 로직 변경 없음)
- `refactor`: 코드 리팩토링
- `test`: 테스트 추가 또는 수정
- `chore`: 유지보수 작업
- `perf`: 성능 개선
- `ci`: CI/CD 변경

#### Scope (선택사항)

- `ota`: OTA 업데이트 로직
- `bluetooth`: Bluetooth 연결
- `ui`: 사용자 인터페이스
- `permissions`: 권한 처리
- `logging`: 로깅 시스템

#### 예시

```bash
# 기능 추가
git commit -m "feat(ota): 다중 디바이스 OTA 지원 추가"

# 버그 수정
git commit -m "fix(bluetooth): BLE 연결 타임아웃 문제 해결"

# 문서
git commit -m "docs: README에 설치 지침 업데이트"

# 리팩토링
git commit -m "refactor(ui): 연결 프래그먼트를 별도 클래스로 분리"
```

#### 여러 줄 커밋

```bash
git commit -m "feat(ota): 펌웨어 검증 구현

- 체크섬 검증 추가
- 펌웨어 파일 형식 검증
- 손상된 파일에 대한 에러 처리 추가

Closes #123"
```

### 커밋 모범 사례

- **원자적 커밋 유지**: 커밋당 하나의 논리적 변경
- **설명적인 메시지 작성**: 무엇을, 왜 했는지 설명 (어떻게가 아님)
- **이슈 참조**: `Closes #123` 또는 `Fixes #456` 사용
- **작은 커밋 유지**: 리뷰와 되돌리기가 쉬움

---

## Pull Request 절차

### 제출 전

✅ **체크리스트**:

- [ ] 코드가 프로젝트 스타일 가이드라인 준수
- [ ] 모든 테스트 통과
- [ ] 새 기능에 대한 새 테스트 추가
- [ ] 문서 업데이트
- [ ] main 브랜치와 충돌 없음
- [ ] 변경사항 자체 검토 완료
- [ ] 의미있는 커밋 메시지 작성

### Pull Request 생성

1. **GitHub에서 fork로 이동**
2. **"New Pull Request" 클릭**
3. **브랜치 선택**:
   - Base: `main` (upstream 저장소)
   - Compare: `feature/your-feature` (본인 브랜치)

### Pull Request 템플릿

```markdown
## 설명
변경사항에 대한 간략한 설명.

## 변경 유형
- [ ] 버그 수정 (이슈를 수정하는 비-파괴적 변경)
- [ ] 새 기능 (기능을 추가하는 비-파괴적 변경)
- [ ] 파괴적 변경 (기존 기능이 예상대로 작동하지 않게 만드는 수정 또는 기능)
- [ ] 문서 업데이트

## 관련 이슈
Closes #(이슈 번호)

## 테스트 방법
실행한 테스트와 그 결과를 설명하세요.

## 스크린샷 (해당되는 경우)
변경사항 설명에 도움이 되는 스크린샷 추가.

## 체크리스트
- [ ] 내 코드가 스타일 가이드라인을 따릅니다
- [ ] 자체 검토를 수행했습니다
- [ ] 특히 이해하기 어려운 부분에 주석을 달았습니다
- [ ] 문서에 상응하는 변경사항을 반영했습니다
- [ ] 내 변경사항이 새로운 경고를 생성하지 않습니다
- [ ] 내 수정이 효과적이거나 기능이 작동함을 증명하는 테스트를 추가했습니다
- [ ] 신규 및 기존 단위 테스트가 내 변경사항으로 로컬에서 통과합니다
```

### 리뷰 프로세스

1. **메인테이너 리뷰**: 프로젝트 메인테이너가 PR 검토
2. **피드백**: 댓글이나 요청된 변경사항 처리
3. **업데이트**: 필요시 추가 커밋 푸시
4. **승인**: 승인되면 PR이 병합됩니다

### 병합 후

```bash
# 로컬 main 브랜치 업데이트
git checkout main
git pull upstream main

# 기능 브랜치 삭제
git branch -d feature/your-feature-name
git push origin --delete feature/your-feature-name
```

---

## 테스트 가이드라인

### 단위 테스트

위치: `otasdk/src/test/java/`

```kotlin
@Test
fun `유효한 파일로 OTA 파일 검증 테스트`() {
    // Arrange
    val validFile = createMockFirmwareFile()
    val validator = OTAFileValidator()

    // Act
    val result = validator.validate(validFile)

    // Assert
    assertTrue(result.isValid)
    assertNull(result.error)
}
```

### 계측 테스트

위치: `otasdk/src/androidTest/java/`

```kotlin
@Test
fun testBluetoothConnection() {
    // 실제 디바이스에서 Bluetooth 연결 테스트
    val scenario = launchActivity<MainActivity>()

    onView(withId(R.id.btn_connect))
        .perform(click())

    onView(withId(R.id.tv_status))
        .check(matches(withText("연결됨")))
}
```

### 테스트 커버리지

- **80% 이상 코드 커버리지** 목표
- 중요 경로 집중: OTA 로직, Bluetooth 통신
- 엣지 케이스 및 에러 시나리오 테스트

### 테스트 실행

```bash
# 모든 단위 테스트
./gradlew test

# 특정 테스트 클래스
./gradlew test --tests "OTAManagerTest"

# 모든 계측 테스트
./gradlew connectedAndroidTest

# 커버리지 리포트 생성
./gradlew jacocoTestReport
```

---

## 문서화

### 코드 문서화

- **Public API**: 항상 KDoc/JavaDoc으로 문서화
- **복잡한 로직**: 인라인 주석 추가
- **TODO**: 향후 작업에 `// TODO: 설명` 사용
- **FIXME**: 알려진 이슈에 `// FIXME: 설명` 사용

### README 업데이트

다음의 경우 `README.md` 업데이트:
- 새 기능 추가
- 설치 프로세스 변경
- 요구사항 업데이트
- 사용 예제 수정

### CHANGELOG

버전 업데이트 시 변경사항 문서화:

```markdown
## [1.8.2] - 2025-XX-XX

### Added
- 새 디바이스 타입 XYZ 지원

### Changed
- BLE 연결 안정성 개선

### Fixed
- Android 14 권한 이슈 수정 (#123)

### Deprecated
- 이전 재연결 방식 (v2.0에서 제거 예정)
```

---

## 질문이 있으신가요?

기여에 대한 질문이 있다면:

1. **기존 문서 확인**:
   - [ONBOARDING_KR.md](ONBOARDING_KR.md)
   - [README.md](README.md)
   - [공식 문서](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/index.html)

2. **기존 이슈 검색**: 같은 질문을 한 사람이 있을 수 있습니다

3. **새 이슈 생성**: "Question" 템플릿 사용

4. **메인테이너 연락**: GitHub Issues 또는 이메일을 통해

---

## 인정

기여자는 다음에서 인정받습니다:
- 릴리스 노트
- 기여자 목록
- 중요한 기여에 대한 특별 감사

Android JL OTA SDK에 기여해주셔서 감사합니다! 🎉

---

마지막 업데이트: 2025-11-09
