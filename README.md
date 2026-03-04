<div align="center">

# 🐘 Learn Gradle

### *The Definitive Guide to Mastering Gradle Build Automation*

[![Gradle](https://img.shields.io/badge/Gradle-8.x-02303A?style=for-the-badge&logo=gradle&logoColor=white)](https://gradle.org)
[![Java](https://img.shields.io/badge/Java-21+-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://openjdk.org)
[![Kotlin DSL](https://img.shields.io/badge/Kotlin_DSL-Supported-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

---

**A comprehensive, hands-on reference** for everything Gradle — from installation and project creation to advanced dependency management, plugin development, and multi-project architectures.

[Standards](#standards--best-practices-quick-ref) •
[Installation](#installation) •
[Quick Start](#quick-start) •
[Project Types](#available-project-types) •
[Dependencies](#dependency-management) •
[Plugins](#plugin-management) •
[Best Practices](#best-practices)

---

</div>

## 🔝 Standards & Best Practices (Quick Ref)

> [!IMPORTANT]
> Use this section as a frequent reference for maintaining high-quality Gradle builds.

| Category | Standard / Best Practice |
|:---|:---|
| **Execution** | Always use the **Gradle Wrapper** (`./gradlew`) instead of a local `gradle` installation. |
| **DSL** | Prefer **Kotlin DSL** (`.gradle.kts`) over Groovy for type safety and IDE support. |
| **Versions** | Manage all dependency and plugin versions in the **Version Catalog** (`libs.versions.toml`). |
| **Logic** | Share build logic via **Convention Plugins** (in `buildSrc` or included builds); avoid `subprojects {}`. |
| **Deps** | Use `implementation` by default; only use `api` if the dependency is part of your library's public API. |
| **Java** | Use **Java Toolchains** to ensure reproducible builds regardless of the local JDK version. |
| **Performance** | Enable `org.gradle.parallel`, `org.gradle.caching`, and `org.gradle.configuration-cache`. |

---

## 📖 Table of Contents

<details>
<summary><b>Click to expand full table of contents</b></summary>

- [🔝 Standards & Best Practices](#standards--best-practices-quick-ref)
- [🚀 Installation](#installation)
  - [Using SDKMAN!](#using-sdkman-recommended)
  - [Using Homebrew](#using-homebrew-macos)
  - [Manual Installation](#manual-installation)
  - [Verify Installation](#verify-installation)
- [⚡ Quick Start](#quick-start)
  - [Interactive Mode](#interactive-mode)
  - [Non-Interactive Mode](#non-interactive-mode)
  - [Available Project Types](#available-project-types)
- [🚀 Sample Projects](#sample-projects)
- [🏗️ Project Structure](#project-structure)
  - [Single Project Layout](#single-project-layout)
  - [Multi-Project Layout](#multi-project-layout)
  - [Key Files Explained](#key-files-explained)
- [📦 Single vs Multi-Project](#single-vs-multi-project)
  - [Single Project Setup](#single-project)
  - [Multi-Project Setup](#multi-project)
  - [Composite Builds](#composite-builds)
- [🔗 Dependency Management](#dependency-management)
  - [Declaring Dependencies](#declaring-dependencies)
  - [Dependency Configurations](#dependency-configurations)
  - [Version Catalogs](#version-catalogs-libsversiontoml)
  - [BOMs & Platforms](#boms--platforms)
  - [Transitive Dependencies](#transitive-dependencies)
  - [Dependency Locking](#dependency-locking)
- [🔌 Plugin Management](#plugin-management)
  - [Applying Plugins](#applying-plugins)
  - [Plugin DSL](#plugin-dsl)
  - [Plugin Management in Settings](#plugin-management-in-settings)
  - [🧩 Convention Plugins](#convention-plugins)
  - [Plugin Repositories](#plugin-repositories)
- [🏷️ Version Management](#version-management)
  - [Gradle Wrapper](#gradle-wrapper)
  - [Java Toolchains](#java-toolchains)
  - [Managing Versions Centrally](#managing-versions-centrally)
- [⚙️ Task Management](#task-management)
  - [Built-in Tasks](#built-in-tasks)
  - [Custom Tasks](#custom-tasks)
  - [Task Dependencies](#task-dependencies)
- [🧪 Testing](#testing)
- [🚀 Build Optimization](#build-optimization)
- [📝 Best Practices](#best-practices)
- [📚 Resources](#resources)

</details>

---

## 🚀 Installation

### Using SDKMAN! (Recommended)

> [!TIP]
> **SDKMAN!** is the easiest way to manage multiple Gradle versions side by side.

```bash
# Install SDKMAN! (if not already installed)
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"

# Install latest Gradle
sdk install gradle

# Install a specific version
sdk install gradle 8.13

# Switch between versions
sdk use gradle 8.13
```

### Using Homebrew (macOS)

```bash
brew install gradle
```

### Using Scoop (Windows)

```powershell
scoop install gradle
```

### Manual Installation

```bash
# 1. Download the latest distribution
wget https://services.gradle.org/distributions/gradle-8.13-bin.zip

# 2. Extract to your preferred location
sudo mkdir /opt/gradle
sudo unzip -d /opt/gradle gradle-8.13-bin.zip

# 3. Add to PATH (add to ~/.bashrc or ~/.zshrc)
export GRADLE_HOME=/opt/gradle/gradle-8.13
export PATH=$GRADLE_HOME/bin:$PATH
```

### Verify Installation

```bash
$ gradle --version

------------------------------------------------------------
Gradle 8.13
------------------------------------------------------------

Build time:    2025-02-17 13:40:30 UTC
Revision:      ...
Kotlin:        2.1.10
Groovy:        3.0.22
Ant:           Apache Ant(TM) version 1.10.15
Launcher JVM:  21.0.6
Daemon JVM:    /usr/lib/jvm/java-21 (no JDK specified, using current Java home)
OS:            Linux 6.x amd64
```

---

## ⚡ Quick Start

### 🎮 Interactive Mode

The interactive mode guides you step-by-step through project creation with prompts:

```bash
# Create a new project directory
mkdir my-project && cd my-project

# Run interactive init
gradle init
```

You'll be prompted to configure:

```
Select type of build to generate:
  1: Application
  2: Library
  3: Gradle plugin
  4: Basic (build structure only)
Enter selection (default: Application) [1..4]

Select implementation language:
  1: Java
  2: Kotlin
  3: Groovy
  4: Scala
  5: C++
  6: Swift
Enter selection (default: Java) [1..6]

Enter target Java version (min: 7, default: 21):

Select build script DSL:
  1: Kotlin
  2: Groovy
Enter selection (default: Kotlin) [1..2]

Select test framework:
  1: JUnit Jupiter
  2: TestNG
  3: Spock
  4: JUnit 4
Enter selection (default: JUnit Jupiter) [1..4]

Project name (default: my-project):
Source package (default: org.example):
```

### 🤖 Non-Interactive Mode

For automation, CI/CD pipelines, and scripting — create projects with a single command:

```bash
# Java Application (Kotlin DSL)
gradle init \
  --type java-application \
  --dsl kotlin \
  --test-framework junit-jupiter \
  --project-name my-app \
  --package com.example \
  --java-version 21 \
  --no-comments \
  --no-interaction

# Java Library (Groovy DSL)
gradle init \
  --type java-library \
  --dsl groovy \
  --test-framework junit-jupiter \
  --project-name my-lib \
  --package com.example.lib \
  --java-version 21 \
  --no-interaction

# Kotlin Application
gradle init \
  --type kotlin-application \
  --dsl kotlin \
  --test-framework kotlintest \
  --project-name my-kotlin-app \
  --package com.example \
  --no-interaction

# Minimal / Basic project (just the Gradle structure)
gradle init \
  --type basic \
  --dsl kotlin \
  --project-name my-basic-project \
  --no-interaction
```

### Available Project Types

| Type | `--type` Flag | Description |
|:-----|:-------------|:------------|
| 🖥️ **Application** | `java-application` | Executable Java app with `main()` entry point |
| 📚 **Library** | `java-library` | Reusable Java library JAR |
| 🧩 **Gradle Plugin** | `java-gradle-plugin` | Custom Gradle plugin project |
| 📦 **Basic** | `basic` | Minimal Gradle structure only |
| 🟣 **Kotlin App** | `kotlin-application` | Executable Kotlin application |
| 📖 **Kotlin Library** | `kotlin-library` | Reusable Kotlin library |
| 🟢 **Groovy App** | `groovy-application` | Executable Groovy application |
| 📗 **Groovy Library** | `groovy-library` | Reusable Groovy library |
| 🔷 **Scala App** | `scala-application` | Executable Scala application |
| 📘 **Scala Library** | `scala-library` | Reusable Scala library |
| ⚙️ **C++ App** | `cpp-application` | Executable C++ application |
| 📙 **C++ Library** | `cpp-library` | Reusable C++ library |

---

## 🚀 Sample Projects

> [!TIP]
> This repository includes functional sample projects demonstrating the best practices described here.

| Sample | Description | Key Features |
|:---|:---|:---|
| [**Single Project**](./samples/single-project) | Minimal standalone application | Kotlin DSL, JUnit 5, Standard Layout |
| [**Multi Project**](./samples/multi-project) | Professional modular architecture | `buildSrc` Convention Plugins, Version Catalogs, Inter-module dependencies |

---

## 🏗️ Project Structure

### Single Project Layout

Typically used for standalone applications or simple libraries.

```
my-single-project/
├── 📄 .gitignore                    # Git ignore rules
├── 📄 gradle.properties             # Project-wide settings
├── 📄 settings.gradle.kts           # Defines project name
├── 📁 gradle/                       # Gradle wrapper files
│   └── 📁 wrapper/
├── 📄 gradlew                       # Wrapper script
└── 📁 app/                          # Main module
    ├── 📄 build.gradle.kts          # Dependencies & tasks
    └── 📁 src/
        ├── 📁 main/                 # App source
        └── 📁 test/                 # Test source
```

### Multi-Project Layout

Designed for modularized systems with shared logic.

```
my-multi-project/
├── 📁 .github/                      # CI/CD workflows
├── 📄 .gitignore                    # Shared ignore rules
├── 📄 settings.gradle.kts           # Includes all submodules
├── 📁 gradle/
│   └── 📄 libs.versions.toml        # Shared Version Catalog
├── 📁 buildSrc/                     # Shared Convention Plugins
│   └── 📁 src/main/kotlin/
├── 📁 api/                          # Interface module
├── 📁 core/                         # Implementation module
└── 📁 app/                          # Executable module
    └── 📄 build.gradle.kts
```

### Key Files Explained

<table>
<tr>
<th>File</th>
<th>Purpose</th>
</tr>
<tr>
<td><code>settings.gradle.kts</code></td>
<td>

**Root settings** — defines the project name, includes subprojects, and configures plugin resolution.
```kotlin
rootProject.name = "my-project"
include("app", "core", "api")
```

</td>
</tr>
<tr>
<td><code>build.gradle.kts</code></td>
<td>

**Build script** — configures plugins, dependencies, tasks, and build logic for a specific subproject.
```kotlin
plugins {
    application
}
dependencies {
    implementation(libs.guava)
}
```

</td>
</tr>
<tr>
<td><code>gradle.properties</code></td>
<td>

**Project properties** — JVM args, feature flags, and project-wide settings.
```properties
org.gradle.jvmargs=-Xmx2g -XX:+UseParallelGC
org.gradle.parallel=true
org.gradle.caching=true
```

</td>
</tr>
<tr>
<td><code>libs.versions.toml</code></td>
<td>

**Version catalog** — centralized dependency version management.
```toml
[versions]
guava = "33.4.0-jre"

[libraries]
guava = { module = "com.google.guava:guava", version.ref = "guava" }
```

</td>
</tr>
</table>

---

## 📦 Single vs Multi-Project

### 📌 Single Project

A single project build has **one subproject** (usually called `app` or the project name itself).

> [!TIP]
> Use a single project when building a **standalone application** or a **single library** with no internal modularity needs.

**`settings.gradle.kts`**
```kotlin
rootProject.name = "my-app"
include("app")
```

**`app/build.gradle.kts`**
```kotlin
plugins {
    application
}

repositories {
    mavenCentral()
}

dependencies {
    implementation(libs.guava)
    testImplementation(libs.junit.jupiter)
}

application {
    mainClass.set("com.example.App")
}
```

#### ✅ Best Practices for Single Projects

| Practice | Description |
|:---------|:------------|
| 🎯 **Keep `build.gradle.kts` focused** | Only declare what the project needs |
| 📦 **Use the version catalog** | Even for a single project — it makes upgrades easier |
| 🧰 **Use the Gradle Wrapper** | Always commit `gradlew` and `gradle/wrapper/` to version control |
| 🏷️ **Set Java toolchains** | Decouple build from the local JDK installation |
| 🚫 **No root `build.gradle.kts`** | Avoid placing build logic in the root project |

---

### 🏢 Multi-Project

A multi-project build contains **multiple subprojects** (modules) under a single root. The subprojects can depend on each other.

> [!IMPORTANT]
> Multi-project builds are ideal for **microservices, modular monoliths, or libraries with separate API/implementation modules**.

**`settings.gradle.kts`**
```kotlin
rootProject.name = "my-platform"

include(
    "core",
    "api",
    "app",
    "common:utils",       // Nested subproject
    "common:models"       // Nested subproject
)
```

**`core/build.gradle.kts`**
```kotlin
plugins {
    `java-library`
}

dependencies {
    api(project(":common:models"))
    implementation(project(":common:utils"))
}
```

**`app/build.gradle.kts`**
```kotlin
plugins {
    application
}

dependencies {
    implementation(project(":core"))
    implementation(project(":api"))
}

application {
    mainClass.set("com.example.App")
}
```

#### ✅ Best Practices for Multi-Project Builds

| Practice | Description |
|:---------|:------------|
| 🏗️ **Use convention plugins** | Share build logic via `buildSrc` or included builds |
| 🔗 **Prefer `api` vs `implementation`** | Use `api` for transitive exposure, `implementation` for internal use |
| 📁 **Logical grouping** | Group related modules under directories (e.g., `common/utils`) |
| 🚫 **Avoid cross-project configurations** | Don't use `allprojects {}` or `subprojects {}` — use convention plugins |
| 📦 **Version catalog for all modules** | Single source of truth for dependency versions |
| 🔄 **Enable parallel execution** | Add `org.gradle.parallel=true` to `gradle.properties` |

---

### 🧱 Composite Builds

Composite builds let you **include independent builds** as dependencies of another build. This is great for developing libraries alongside your application.

```kotlin
// settings.gradle.kts
rootProject.name = "my-app"

// Include an external library build
includeBuild("../my-library") {
    dependencySubstitution {
        substitute(module("com.example:my-library"))
            .using(project(":"))
    }
}

include("app")
```

> [!NOTE]
> With composite builds, changes to `my-library` are **instantly reflected** in `my-app` without publishing — perfect for local development.

---

## 🔗 Dependency Management

### Declaring Dependencies

```kotlin
// build.gradle.kts
dependencies {
    // External dependencies (from repositories)
    implementation("com.google.guava:guava:33.4.0-jre")

    // Using version catalog (recommended)
    implementation(libs.guava)

    // Project dependencies (multi-project)
    implementation(project(":core"))

    // File dependencies (local JARs)
    implementation(files("libs/custom.jar"))
    implementation(fileTree("libs") { include("*.jar") })
}
```

### Dependency Configurations

```
┌─────────────────────────────────────────────────────────┐
│                  Dependency Configurations               │
├──────────────────────┬──────────────────────────────────┤
│   Configuration      │   When Available                 │
├──────────────────────┼──────────────────────────────────┤
│ implementation       │ Compile + Runtime (not exposed)  │
│ api                  │ Compile + Runtime (exposed)      │
│ compileOnly          │ Compile only (e.g. Lombok)       │
│ runtimeOnly          │ Runtime only (e.g. JDBC drivers) │
│ testImplementation   │ Test compile + runtime           │
│ testCompileOnly      │ Test compile only                │
│ testRuntimeOnly      │ Test runtime only                │
│ annotationProcessor  │ Annotation processing (compile)  │
└──────────────────────┴──────────────────────────────────┘
```

> [!WARNING]
> **Avoid using `compile` and `runtime`** — these are deprecated since Gradle 7. Always use `implementation` or `api` instead.

### When to Use `api` vs `implementation`

```kotlin
// java-library plugin required for `api`
plugins {
    `java-library`
}

dependencies {
    // ✅ api — Types from this dependency appear in YOUR public API
    //    Consumers of your library will also see this dependency
    api("org.apache.commons:commons-lang3:3.17.0")

    // ✅ implementation — Used internally only
    //    Consumers of your library will NOT see this dependency
    implementation("com.google.guava:guava:33.4.0-jre")
}
```

### 📋 Version Catalogs (`libs.versions.toml`)

> [!TIP]
> Version catalogs are the **recommended** way to manage dependency versions since Gradle 7.0+. They provide a single source of truth and are accessible across all subprojects.

**`gradle/libs.versions.toml`**
```toml
[versions]
kotlin = "2.1.10"
spring-boot = "3.4.3"
guava = "33.4.0-jre"
junit-jupiter = "5.11.4"
jackson = "2.18.3"
lombok = "1.18.36"
slf4j = "2.0.16"

[libraries]
# Format: alias = { module = "group:artifact", version.ref = "version-key" }
guava = { module = "com.google.guava:guava", version.ref = "guava" }
junit-jupiter = { module = "org.junit.jupiter:junit-jupiter", version.ref = "junit-jupiter" }
jackson-databind = { module = "com.fasterxml.jackson.core:jackson-databind", version.ref = "jackson" }
jackson-kotlin = { module = "com.fasterxml.jackson.module:jackson-module-kotlin", version.ref = "jackson" }
lombok = { module = "org.projectlombok:lombok", version.ref = "lombok" }
slf4j-api = { module = "org.slf4j:slf4j-api", version.ref = "slf4j" }

[bundles]
# Group related dependencies together
jackson = ["jackson-databind", "jackson-kotlin"]
testing = ["junit-jupiter"]

[plugins]
kotlin-jvm = { id = "org.jetbrains.kotlin.jvm", version.ref = "kotlin" }
spring-boot = { id = "org.springframework.boot", version.ref = "spring-boot" }
```

**Usage in `build.gradle.kts`:**
```kotlin
plugins {
    alias(libs.plugins.kotlin.jvm)
    alias(libs.plugins.spring.boot)
}

dependencies {
    implementation(libs.guava)
    implementation(libs.bundles.jackson)  // Adds all Jackson dependencies
    compileOnly(libs.lombok)
    annotationProcessor(libs.lombok)
    testImplementation(libs.bundles.testing)
}
```

### 🌐 BOMs & Platforms

Use BOMs (Bill of Materials) to manage groups of related dependency versions:

```kotlin
dependencies {
    // Import a BOM — all dependencies in this BOM will have their versions managed
    implementation(platform("org.springframework.boot:spring-boot-dependencies:3.4.3"))

    // Now you can omit versions for any dependency defined in the BOM
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")
    testImplementation("org.springframework.boot:spring-boot-starter-test")

    // Enforce a BOM (overrides even higher transitive versions)
    implementation(enforcedPlatform("org.springframework.cloud:spring-cloud-dependencies:2024.0.0"))
}
```

### Transitive Dependencies

```kotlin
dependencies {
    // Exclude a transitive dependency
    implementation("com.example:my-lib:1.0") {
        exclude(group = "org.unwanted", module = "unwanted-lib")
    }

    // Force a specific version (use with caution)
    implementation("com.google.guava:guava") {
        version {
            strictly("33.4.0-jre")
        }
    }
}

// See all dependencies
// Run: ./gradlew dependencies
// Run: ./gradlew dependencies --configuration runtimeClasspath
```

### 🔒 Dependency Locking

Lock file-based dependency locking ensures **reproducible builds**:

```kotlin
// Enable dependency locking for all configurations
dependencyLocking {
    lockAllConfigurations()
}
```

```bash
# Generate lock files
./gradlew dependencies --write-locks

# Update lock files after changing dependencies
./gradlew dependencies --update-locks com.google.guava:guava
```

---

## 🔌 Plugin Management

### Applying Plugins

```kotlin
// ✅ Modern plugins block (recommended)
plugins {
    java                                    // Core Gradle plugin (no version needed)
    application                             // Core Gradle plugin
    `java-library`                          // Core Gradle plugin
    id("com.github.johnrengelman.shadow") version "8.1.1"   // Community plugin
    alias(libs.plugins.kotlin.jvm)          // From version catalog
}
```

### Plugin DSL

> [!WARNING]
> **Avoid the legacy `apply plugin:` syntax** — always use the `plugins { }` block for better performance and type-safe accessors.

```kotlin
// ❌ Legacy (avoid)
apply(plugin = "java")

// ✅ Modern (preferred)
plugins {
    java
}
```

### Plugin Management in Settings

Centralize plugin version management in `settings.gradle.kts`:

```kotlin
// settings.gradle.kts
pluginManagement {
    repositories {
        gradlePluginPortal()  // Default plugin repository
        mavenCentral()
        google()              // For Android plugins
    }

    // Set default versions for plugins
    plugins {
        id("org.jetbrains.kotlin.jvm") version "2.1.10"
        id("com.github.johnrengelman.shadow") version "8.1.1"
    }
}

rootProject.name = "my-project"
include("app")
```

### 🧩 Convention Plugins

Convention plugins are the **recommended approach** to share build logic across subprojects, replacing `allprojects {}` and `subprojects {}`.

**Option A: Using `buildSrc`**

```
my-project/
├── buildSrc/
│   ├── build.gradle.kts
│   └── src/main/kotlin/
│       ├── java-conventions.gradle.kts    # Common Java config
│       └── testing-conventions.gradle.kts  # Common test config
├── settings.gradle.kts
├── app/
│   └── build.gradle.kts
└── core/
    └── build.gradle.kts
```

**`buildSrc/build.gradle.kts`**
```kotlin
plugins {
    `kotlin-dsl`
}

repositories {
    mavenCentral()
    gradlePluginPortal()
}
```

**`buildSrc/src/main/kotlin/java-conventions.gradle.kts`**
```kotlin
plugins {
    java
}

repositories {
    mavenCentral()
}

java {
    toolchain {
        languageVersion.set(JavaLanguageVersion.of(21))
    }
}

tasks.withType<Test> {
    useJUnitPlatform()
}
```

**`app/build.gradle.kts`** — now super clean:
```kotlin
plugins {
    id("java-conventions")   // Apply convention plugin
    application
}

dependencies {
    implementation(project(":core"))
}
```

### Plugin Repositories

```kotlin
// settings.gradle.kts
pluginManagement {
    repositories {
        gradlePluginPortal()    // Gradle Plugin Portal (default)
        mavenCentral()          // Maven Central
        google()                // Google (Android)
        maven {                 // Custom / private repository
            url = uri("https://nexus.example.com/repository/gradle-plugins/")
            credentials {
                username = providers.gradleProperty("nexusUser").get()
                password = providers.gradleProperty("nexusPassword").get()
            }
        }
    }
}
```

---

## 🏷️ Version Management

### 🔄 Gradle Wrapper

The Gradle Wrapper ensures **everyone uses the same Gradle version**. Always commit it to source control.

```bash
# Generate the wrapper (run from a Gradle-installed environment)
gradle wrapper --gradle-version 8.13 --distribution-type bin

# Upgrade an existing wrapper
./gradlew wrapper --gradle-version 8.13

# Verify the wrapper version
./gradlew --version
```

**`gradle/wrapper/gradle-wrapper.properties`**
```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-8.13-bin.zip
networkTimeout=10000
validateDistributionUrl=true
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

> [!IMPORTANT]
> **Always commit these files to version control:**
> - `gradlew` (Unix wrapper script)
> - `gradlew.bat` (Windows wrapper script)
> - `gradle/wrapper/gradle-wrapper.jar`
> - `gradle/wrapper/gradle-wrapper.properties`

### ☕ Java Toolchains

Decouple your build from the locally installed JDK:

```kotlin
// build.gradle.kts
java {
    toolchain {
        languageVersion.set(JavaLanguageVersion.of(21))
        vendor.set(JvmVendorSpec.ADOPTIUM)  // Optional: specify vendor
    }
}
```

```kotlin
// settings.gradle.kts — auto-download JDKs
plugins {
    id("org.gradle.toolchains.foojay-resolver-convention") version "0.9.0"
}
```

### Managing Versions Centrally

Use `gradle.properties` for project-wide version properties:

```properties
# gradle.properties
group=com.example
version=1.0.0-SNAPSHOT

# Framework versions (when not using version catalog)
springBootVersion=3.4.3
micronautVersion=4.7.6
```

Use version catalogs (`libs.versions.toml`) for dependency and plugin versions — see the [Version Catalogs](#-version-catalogs-libsversiontoml) section above.

---

## ⚙️ Task Management

### Built-in Tasks

```bash
# List all available tasks
./gradlew tasks --all

# Common lifecycle tasks
./gradlew clean                # Deletes the build directory
./gradlew build                # Assembles and tests the project
./gradlew test                 # Runs all tests
./gradlew check                # Runs all verification tasks
./gradlew assemble             # Compiles and packages (no tests)
./gradlew jar                  # Builds the JAR file
./gradlew run                  # Runs the application (application plugin)

# Dependency insight
./gradlew dependencies                             # Full dependency tree
./gradlew dependencies --configuration runtimeClasspath  # Specific config
./gradlew dependencyInsight --dependency guava      # Info about a dependency

# Build information
./gradlew projects             # List all subprojects
./gradlew properties           # List all project properties
./gradlew model                # Display project model
```

### Custom Tasks

```kotlin
// build.gradle.kts

// Simple task
tasks.register("hello") {
    group = "Custom"
    description = "Prints a greeting"
    doLast {
        println("Hello from Gradle!")
    }
}

// Typed task with inputs/outputs (cacheable)
abstract class GenerateReport : DefaultTask() {
    @get:InputFile
    abstract val inputFile: RegularFileProperty

    @get:OutputFile
    abstract val outputFile: RegularFileProperty

    @TaskAction
    fun generate() {
        val data = inputFile.get().asFile.readText()
        outputFile.get().asFile.writeText("Report: $data")
    }
}

tasks.register<GenerateReport>("generateReport") {
    inputFile.set(file("data/input.txt"))
    outputFile.set(layout.buildDirectory.file("reports/output.txt"))
}
```

### Task Dependencies

```kotlin
tasks.register("fullBuild") {
    dependsOn("clean", "build", "generateReport")
    finalizedBy("publishReport")

    doLast {
        println("Full build completed!")
    }
}

// Task ordering (without dependency)
tasks.named("test") {
    mustRunAfter("clean")           // If both run, test runs after clean
    shouldRunAfter("compileJava")   // Soft ordering hint
}
```

---

## 🧪 Testing

```kotlin
// build.gradle.kts
dependencies {
    testImplementation(libs.junit.jupiter)
    testRuntimeOnly("org.junit.platform:junit-platform-launcher")
}

tasks.test {
    useJUnitPlatform()

    // Parallel test execution
    maxParallelForks = Runtime.getRuntime().availableProcessors() / 2

    // Fail fast
    failFast = true

    // Logging
    testLogging {
        events("passed", "skipped", "failed", "standardOut", "standardError")
        showExceptions = true
        showCauses = true
        showStackTraces = true
        exceptionFormat = org.gradle.api.tasks.testing.logging.TestExceptionFormat.FULL
    }

    // JVM args for tests
    jvmArgs("-XX:+EnableDynamicAgentLoading")

    // Filter tests
    filter {
        includeTestsMatching("com.example.*")
        excludeTestsMatching("*IntegrationTest")
    }
}
```

```bash
# Run all tests
./gradlew test

# Run specific test class
./gradlew test --tests "com.example.MyClassTest"

# Run specific test method
./gradlew test --tests "com.example.MyClassTest.myTestMethod"

# Run tests matching a pattern
./gradlew test --tests "*Integration*"

# Run with verbose output
./gradlew test --info

# Re-run tests (skip cache)
./gradlew test --rerun
```

---

## 🚀 Build Optimization

Add these to your **`gradle.properties`** for faster builds:

```properties
# ──────────────────────────────────────────────
#  Performance Tuning
# ──────────────────────────────────────────────

# Increase JVM heap size for the Gradle daemon
org.gradle.jvmargs=-Xmx4g -XX:+HeapDumpOnOutOfMemoryError \
  -XX:+UseParallelGC -Dfile.encoding=UTF-8

# Enable parallel project execution
org.gradle.parallel=true

# Enable the Gradle build cache
org.gradle.caching=true

# Enable the configuration cache (Gradle 8.1+)
org.gradle.configuration-cache=true

# Run only requested tasks (skip dependent project tasks)
org.gradle.configureondemand=true

# Reduce console output noise
org.gradle.console=auto
org.gradle.logging.level=lifecycle
```

> [!TIP]
> Use `./gradlew --scan` to generate a detailed **Build Scan** — a web-based report showing performance bottlenecks, dependency details, and build insights.

---

## 📝 Best Practices

### ✅ Do

| # | Practice | Why |
|:-:|:---------|:----|
| 1 | **Always use the Gradle Wrapper** | Ensures consistent Gradle versions across team |
| 2 | **Use Kotlin DSL** (`*.gradle.kts`) | Better IDE support, type safety, and refactoring |
| 3 | **Use version catalogs** (`libs.versions.toml`) | Centralized, type-safe dependency management |
| 4 | **Use convention plugins** | Replaces `allprojects`/`subprojects` with clean, reusable logic |
| 5 | **Use `implementation` over `api`** | Reduces compilation classpath and improves build times |
| 6 | **Declare repositories in `settings.gradle.kts`** | Consistent resolution across all subprojects |
| 7 | **Enable parallel & cached builds** | Dramatically faster build times |
| 8 | **Use Java toolchains** | Reproducible builds independent of local JDK |
| 9 | **Define task inputs/outputs** | Enables incremental builds and caching |
| 10 | **Use `dependencyLocking`** | Ensures reproducible dependency resolution |

### ❌ Don't

| # | Anti-Pattern | Better Alternative |
|:-:|:-------------|:-------------------|
| 1 | ~~`allprojects { }`~~ / ~~`subprojects { }`~~ | Convention plugins |
| 2 | ~~`apply plugin: "java"`~~ | `plugins { java }` block |
| 3 | ~~`compile`~~ / ~~`runtime`~~ | `implementation` / `runtimeOnly` |
| 4 | ~~Hardcoded versions~~ in build scripts | Version catalogs |
| 5 | ~~Dynamic versions~~ (`1.+`, `latest.release`) | Fixed or range versions |
| 6 | ~~Build logic in root `build.gradle.kts`~~ | Convention plugins in `buildSrc` |
| 7 | ~~`buildscript { }` block~~ for plugins | `plugins { }` block |
| 8 | ~~Invoking `gradle` directly~~ | Use `./gradlew` wrapper |

### 📁 Recommended `.gitignore`

```gitignore
# Gradle
.gradle/
build/
!gradle-wrapper.jar
!gradle-wrapper.properties

# IDE
.idea/
*.iml
.vscode/
.settings/
.project
.classpath
*.swp
*~

# OS
.DS_Store
Thumbs.db

# Environment
.env
*.local
```

---

## 📚 Resources

### Official Documentation

| Resource | Link |
|:---------|:-----|
| 📘 **Gradle User Manual** | [docs.gradle.org/current/userguide](https://docs.gradle.org/current/userguide/userguide.html) |
| 🚀 **Getting Started Tutorial** | [docs.gradle.org/current/userguide/part1_gradle_init.html](https://docs.gradle.org/current/userguide/part1_gradle_init.html) |
| 🔗 **Dependency Management** | [docs.gradle.org/current/userguide/dependency_management.html](https://docs.gradle.org/current/userguide/dependency_management.html) |
| 📋 **Version Catalogs** | [docs.gradle.org/current/userguide/version_catalogs.html](https://docs.gradle.org/current/userguide/version_catalogs.html) |
| 🔌 **Plugin Development** | [docs.gradle.org/current/userguide/custom_plugins.html](https://docs.gradle.org/current/userguide/custom_plugins.html) |
| 🏗️ **Multi-Project Builds** | [docs.gradle.org/current/userguide/multi_project_builds.html](https://docs.gradle.org/current/userguide/multi_project_builds.html) |
| ⚡ **Build Cache** | [docs.gradle.org/current/userguide/build_cache.html](https://docs.gradle.org/current/userguide/build_cache.html) |
| 🧪 **Testing in Gradle** | [docs.gradle.org/current/userguide/java_testing.html](https://docs.gradle.org/current/userguide/java_testing.html) |
| 📖 **Kotlin DSL Primer** | [docs.gradle.org/current/userguide/kotlin_dsl.html](https://docs.gradle.org/current/userguide/kotlin_dsl.html) |
| 🔄 **Gradle Wrapper** | [docs.gradle.org/current/userguide/gradle_wrapper.html](https://docs.gradle.org/current/userguide/gradle_wrapper.html) |

### Community & Tools

| Resource | Link |
|:---------|:-----|
| 🔍 **Gradle Plugin Portal** | [plugins.gradle.org](https://plugins.gradle.org) |
| 🏪 **Maven Central Search** | [search.maven.org](https://search.maven.org) |
| 📊 **Build Scans** | [scans.gradle.com](https://scans.gradle.com) |
| 💬 **Gradle Forums** | [discuss.gradle.org](https://discuss.gradle.org) |
| 🐙 **Gradle GitHub** | [github.com/gradle/gradle](https://github.com/gradle/gradle) |

---

<div align="center">

### 🎓 Happy Building!

*Made with 💚 for the Gradle community*

[![Gradle](https://img.shields.io/badge/Powered_by-Gradle-02303A?style=flat-square&logo=gradle&logoColor=white)](https://gradle.org)

</div>