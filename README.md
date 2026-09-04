# Spring Boot Gradle Template

A modern Java Spring Boot Gradle multi-module template with integrated PR intelligence, code formatting, linting, and automated PR title validation.

## Features

✨ **1. PR Intelligence for Java Spring Applications**
- Automated PR analysis with complexity metrics
- Smart review checklists
- File change tracking
- Addition/deletion statistics

✨ **2. Pre-commit Hooks for Java Spring**
- Auto-format on commit using Spotless
- Lint validation before commit
- Only processes changed files
- Automatically stages formatted files

✨ **3. Spotless + Google Java Formatter**
- Automatic code style enforcement
- Google Java style guide compliance
- Gradle and Java file formatting
- Trimmed whitespace and consistent line endings

✨ **4. Auto Format/Lint on Git Commit**
- Runs only on changed files
- Formats with Spotless
- Auto-adds formatted files to same commit
- Validates with spotlessCheck

✨ **5. Git Workflow for PR Title Validation**
- Enforces PR title format
- Validates ticket reference format
- Supports: `feature:`, `bugfix:`, `chore:`, `hotfix:`
- Example: `feature: [US1234] - Add user authentication`

## Project Structure

```
spring-gradle-pr-template/
├── gradle.properties              # Gradle configuration
├── settings.gradle                # Module definitions
├── build.gradle                   # Root build configuration
├── quality/spotless/
│   └── backend-spotless.gradle   # Spotless formatter config
├── shared-kernel/                # Shared utilities and base classes
│   └── build.gradle
├── core-module/                  # Core business logic
│   └── build.gradle
├── app/                          # Spring Boot application entry point
│   └── build.gradle
├── .githooks/                    # Git hooks (pre-commit)
│   └── pre-commit
└── README.md
```

## Quick Start

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/marvelousdev0/spring-gradle-pr-template.git
   cd spring-gradle-pr-template
   ```

2. **Install Git hooks (one-time setup)**
   ```bash
   git config core.hooksPath .githooks
   chmod +x .githooks/pre-commit
   ```

3. **Build the project**
   ```bash
   ./gradlew build
   ```

### Development

**Run the application:**
```bash
./gradlew :app:bootRun
```

**Format code (Spotless):**
```bash
./gradlew spotlessApply
```

**Check code formatting:**
```bash
./gradlew spotlessCheck
```

**Run tests:**
```bash
./gradlew test
```

## PR Title Format

All pull requests must follow this format:

```
type: [TICKET] - description
```

### Examples

✅ **Valid PR Titles:**
- `feature: [US1234] - Add user authentication system`
- `bugfix: [BUG567] - Fix null pointer in UserService`
- `chore: [CHORE890] - Update Spring Boot to 3.5.5`
- `hotfix: [HOT123] - Critical fix for production issue`

❌ **Invalid PR Titles:**
- `Add authentication` (missing type and ticket)
- `feature: Add authentication` (missing ticket)
- `feature: [US1234]` (missing description)

### Allowed Types

- **feature:** New functionality or enhancement
- **bugfix:** Bug fixes and corrections
- **chore:** Build, CI/CD, dependencies, documentation
- **hotfix:** Critical fixes requiring immediate deployment

## Code Style

This project enforces **Google Java Style Guide** via Spotless and google-java-format.

### Key Rules

- 4-space indentation
- 100-character line limit (soft)
- Consistent naming conventions
- Proper import organization
- No trailing whitespace
- Files end with newline

### Pre-commit Behavior

When you commit:

1. ✅ Detects changed `.java` and `.gradle` files
2. ✅ Runs `spotlessApply` to format them
3. ✅ Auto-stages formatted files
4. ✅ Validates with `spotlessCheck`
5. ✅ Aborts commit if validation fails

## Technology Stack

- **Java 21**
- **Spring Boot 3.5.5**
- **Gradle 8.14**
- **Spotless 7.0.4** (with google-java-format 1.30.0)
- **Lombok 1.18.38**
- **MapStruct 1.6.3**
- **JUnit 5**

## Configuration Files

### `gradle.properties`

Centralized version management for all dependencies.

### `build.gradle` (Root)

- Applies Spotless to all subprojects
- Defines common dependencies
- Configures dependency management

### Module `build.gradle` Files

Each module (`shared-kernel`, `core-module`, `app`) defines its own dependencies.

## Contributing

1. Create a feature branch following the PR title format
2. Commit your changes (pre-commit hooks will format automatically)
3. Push to GitHub and create a PR
4. Ensure PR title matches required format
5. Wait for PR Intelligence analysis and validation checks
6. Request review and merge

## Troubleshooting

### Pre-commit hook not running

```bash
# Ensure hooks are installed
git config core.hooksPath .githooks
chmod +x .githooks/pre-commit
```

### Spotless check failures

```bash
# Auto-fix formatting issues
./gradlew spotlessApply
```

### PR title validation failing

- Ensure format: `type: [TICKET] - description`
- Use allowed types: `feature`, `bugfix`, `chore`, `hotfix`
- Include ticket reference in square brackets

## References

- [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spotless on GitHub](https://github.com/diffplug/spotless)
- [Gradle Documentation](https://docs.gradle.org/)
