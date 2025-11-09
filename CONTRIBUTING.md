# Contributing to Android JL OTA SDK

Thank you for considering contributing to Android JL OTA SDK! This document provides guidelines and instructions for contributing.

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [Development Process](#development-process)
4. [Coding Standards](#coding-standards)
5. [Commit Guidelines](#commit-guidelines)
6. [Pull Request Process](#pull-request-process)
7. [Testing Guidelines](#testing-guidelines)
8. [Documentation](#documentation)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for all contributors.

### Expected Behavior

- Be respectful and considerate
- Accept constructive criticism gracefully
- Focus on what is best for the project and community
- Show empathy towards other community members

### Unacceptable Behavior

- Harassment or discriminatory language
- Trolling or insulting comments
- Public or private harassment
- Publishing others' private information without permission

---

## Getting Started

### Prerequisites

Before contributing, ensure you have:

1. Read the [ONBOARDING.md](ONBOARDING.md) guide
2. Set up your development environment
3. Successfully built and run the project
4. Familiarized yourself with the codebase

### Finding Issues to Work On

1. Check the [Issues](https://github.com/Jieli-Tech/Android-JL_OTA/issues) page
2. Look for issues labeled:
   - `good first issue` - Perfect for beginners
   - `help wanted` - Community contributions welcome
   - `bug` - Bug fixes needed
   - `enhancement` - New features or improvements

### Claiming an Issue

1. Comment on the issue expressing your interest
2. Wait for maintainer acknowledgment
3. Begin work after confirmation

---

## Development Process

### 1. Fork and Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR_USERNAME/Android-JL_OTA.git
cd Android-JL_OTA

# Add upstream remote
git remote add upstream https://github.com/Jieli-Tech/Android-JL_OTA.git
```

### 2. Create a Branch

```bash
# Update your local main branch
git checkout main
git pull upstream main

# Create a new branch
git checkout -b feature/your-feature-name
# or
git checkout -b fix/issue-description
```

Branch naming conventions:
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation updates
- `refactor/` - Code refactoring
- `test/` - Test additions or modifications
- `chore/` - Maintenance tasks

### 3. Make Changes

- Write clean, maintainable code
- Follow existing code style
- Add comments for complex logic
- Update documentation as needed

### 4. Test Your Changes

```bash
# Run unit tests
./gradlew test

# Run instrumented tests
./gradlew connectedAndroidTest

# Build the project
./gradlew clean build
```

### 5. Commit Your Changes

See [Commit Guidelines](#commit-guidelines) below.

### 6. Push to Your Fork

```bash
git push origin feature/your-feature-name
```

### 7. Create Pull Request

See [Pull Request Process](#pull-request-process) below.

---

## Coding Standards

### Kotlin Style Guide

Follow the [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html):

#### Naming

```kotlin
// Classes: PascalCase
class OTAManager { }

// Functions: camelCase
fun startOtaUpdate() { }

// Properties: camelCase
val deviceName: String

// Constants: UPPER_SNAKE_CASE
const val MAX_RETRY_COUNT = 3

// Private properties: start with underscore (optional)
private val _connectionState = MutableLiveData<ConnectionState>()
```

#### Code Organization

```kotlin
class ExampleClass {
    // 1. Companion object
    companion object {
        const val TAG = "ExampleClass"
    }

    // 2. Properties
    private var state: State = State.IDLE

    // 3. Init blocks
    init {
        // Initialization
    }

    // 4. Public functions
    fun publicMethod() { }

    // 5. Private functions
    private fun privateMethod() { }

    // 6. Inner classes
    inner class InnerClass { }
}
```

#### Code Formatting

- **Indentation**: 4 spaces (no tabs)
- **Line length**: Maximum 120 characters
- **Blank lines**: Use to separate logical sections
- **Braces**: K&R style (opening brace on same line)

```kotlin
// Good
if (condition) {
    doSomething()
} else {
    doSomethingElse()
}

// Bad
if (condition)
{
    doSomething()
}
```

### Java Style Guide

For existing Java code, follow [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html):

```java
// Class declaration
public class OTAManager {
    // Constants
    private static final String TAG = "OTAManager";

    // Member variables
    private int retryCount;

    // Constructor
    public OTAManager() {
        this.retryCount = 0;
    }

    // Methods
    public void startUpdate() {
        // Implementation
    }
}
```

### XML Resources

#### Layout Files

```xml
<!-- Use meaningful IDs -->
<TextView
    android:id="@+id/tv_device_name"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/device_name" />
```

#### String Resources

```xml
<!-- Always use string resources, never hardcode strings -->
<string name="ota_update_success">OTA update completed successfully</string>
<string name="ota_update_failed">OTA update failed: %1$s</string>
```

### Comments

```kotlin
/**
 * Starts the OTA update process for the connected device.
 *
 * @param firmwareFile The firmware file to be uploaded
 * @param callback Callback for update progress and completion
 * @throws IllegalStateException if device is not connected
 */
fun startOtaUpdate(
    firmwareFile: File,
    callback: OTACallback
) {
    // Verify device connection
    if (!isDeviceConnected()) {
        throw IllegalStateException("Device not connected")
    }

    // TODO: Add retry mechanism
    // FIXME: Handle edge case when file is corrupted

    // Begin transfer
    transferFirmware(firmwareFile, callback)
}
```

---

## Commit Guidelines

### Commit Message Format

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

#### Type

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, no logic change)
- `refactor`: Code refactoring
- `test`: Adding or modifying tests
- `chore`: Maintenance tasks
- `perf`: Performance improvements
- `ci`: CI/CD changes

#### Scope (Optional)

- `ota`: OTA update logic
- `bluetooth`: Bluetooth connectivity
- `ui`: User interface
- `permissions`: Permission handling
- `logging`: Logging system

#### Examples

```bash
# Feature
git commit -m "feat(ota): add support for multiple device OTA"

# Bug fix
git commit -m "fix(bluetooth): resolve BLE connection timeout issue"

# Documentation
git commit -m "docs: update installation instructions in README"

# Refactoring
git commit -m "refactor(ui): extract connection fragment to separate class"
```

#### Multi-line Commit

```bash
git commit -m "feat(ota): implement firmware validation

- Add checksum verification
- Validate firmware file format
- Add error handling for corrupted files

Closes #123"
```

### Commit Best Practices

- **Keep commits atomic**: One logical change per commit
- **Write descriptive messages**: Explain what and why, not how
- **Reference issues**: Use `Closes #123` or `Fixes #456`
- **Keep commits small**: Easier to review and revert if needed

---

## Pull Request Process

### Before Submitting

✅ **Checklist**:

- [ ] Code follows project style guidelines
- [ ] All tests pass
- [ ] New tests added for new features
- [ ] Documentation updated
- [ ] No merge conflicts with main branch
- [ ] Self-reviewed the changes
- [ ] Added meaningful commit messages

### Creating a Pull Request

1. **Navigate to your fork** on GitHub
2. **Click "New Pull Request"**
3. **Select branches**:
   - Base: `main` (upstream repository)
   - Compare: `feature/your-feature` (your branch)

### Pull Request Template

```markdown
## Description
Brief description of the changes.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Related Issue
Closes #(issue number)

## How Has This Been Tested?
Describe the tests you ran and their results.

## Screenshots (if applicable)
Add screenshots to help explain your changes.

## Checklist
- [ ] My code follows the style guidelines
- [ ] I have performed a self-review
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
```

### Review Process

1. **Maintainer review**: A project maintainer will review your PR
2. **Feedback**: Address any comments or requested changes
3. **Updates**: Push additional commits if needed
4. **Approval**: Once approved, your PR will be merged

### After Merge

```bash
# Update your local main branch
git checkout main
git pull upstream main

# Delete your feature branch
git branch -d feature/your-feature-name
git push origin --delete feature/your-feature-name
```

---

## Testing Guidelines

### Unit Tests

Location: `otasdk/src/test/java/`

```kotlin
@Test
fun `test OTA file validation with valid file`() {
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

### Instrumented Tests

Location: `otasdk/src/androidTest/java/`

```kotlin
@Test
fun testBluetoothConnection() {
    // Test Bluetooth connection on real device
    val scenario = launchActivity<MainActivity>()

    onView(withId(R.id.btn_connect))
        .perform(click())

    onView(withId(R.id.tv_status))
        .check(matches(withText("Connected")))
}
```

### Test Coverage

- Aim for **80%+ code coverage**
- Focus on critical paths: OTA logic, Bluetooth communication
- Test edge cases and error scenarios

### Running Tests

```bash
# All unit tests
./gradlew test

# Specific test class
./gradlew test --tests "OTAManagerTest"

# All instrumented tests
./gradlew connectedAndroidTest

# Generate coverage report
./gradlew jacocoTestReport
```

---

## Documentation

### Code Documentation

- **Public APIs**: Always document with KDoc/JavaDoc
- **Complex logic**: Add inline comments
- **TODOs**: Use `// TODO: Description` for future work
- **FIXMEs**: Use `// FIXME: Description` for known issues

### README Updates

Update `README.md` when:
- Adding new features
- Changing installation process
- Updating requirements
- Modifying usage examples

### CHANGELOG

Document changes in version updates:

```markdown
## [1.8.2] - 2025-XX-XX

### Added
- Support for new device type XYZ

### Changed
- Improved BLE connection stability

### Fixed
- Fixed issue with Android 14 permissions (#123)

### Deprecated
- Old reconnection method (will be removed in v2.0)
```

---

## Questions?

If you have questions about contributing:

1. **Check existing documentation**:
   - [ONBOARDING.md](ONBOARDING.md)
   - [README.md](README.md)
   - [Official Docs](https://doc.zh-jieli.com/Apps/Android/ota/zh-cn/master/index.html)

2. **Search existing issues**: Someone may have asked the same question

3. **Create a new issue**: Use the "Question" template

4. **Contact maintainers**: Via GitHub Issues or email

---

## Recognition

Contributors will be recognized in:
- Release notes
- Contributors list
- Special acknowledgments for significant contributions

Thank you for contributing to Android JL OTA SDK! 🎉

---

Last Updated: 2025-11-09
