Here’s a **practical, step-by-step Gradle build tutorial for a Java developer** — from setup to building and running a Java application:

---

## 📌 1. What Is Gradle?

Gradle is a modern build automation tool used for compiling, testing, packaging, and managing dependencies in Java projects. It uses a Groovy- or Kotlin-based DSL instead of XML (like Maven), which makes build scripts more expressive and concise. ([docs.gradle.org][1])

---

## 📥 2. Prerequisites

Before starting:

* **Java JDK installed** (version 8+).
* **Gradle installed**, or you can use the **Gradle Wrapper** (recommended). ([Gradle][2])

You can verify installation:

```bash
gradle --version
```

---

## 📁 3. Initialize a Gradle Java Project

### **A. Using the CLI (command line)**

1. Create a new directory and go into it:

```bash
mkdir MyGradleApp
cd MyGradleApp
```

2. Initialize a Java application with defaults:

```bash
gradle init --type java-application
```

Follow the prompts:

* Select **Application**
* Choose **Java**
* Pick **Groovy** or **Kotlin** for the build script
* Choose defaults for everything else

This generates the Gradle project structure including `build.gradle`, `settings.gradle`, the wrapper files (`gradlew`), and standard Java directories (`src/main/java`, `src/test/java`). ([docs.gradle.org][3])

---

## 📄 4. Understand the Project Structure

Typical generated structure:

```
MyGradleApp/
├── build.gradle
├── settings.gradle
├── gradlew
├── gradlew.bat
├── gradle/
│   └── wrapper/
├── src/
│   ├── main/java/     ← Your application code
│   └── test/java/     ← Unit tests
```

* **build.gradle** — your build configuration file
* **settings.gradle** — defines the project name
* **gradlew / gradlew.bat** — the Gradle Wrapper scripts ([Medium][4])

---

## 🛠 5. Example `build.gradle` (Groovy)

Here’s a simple build script for a Java app:

```groovy
plugins {
    id 'java'
    id 'application'
}

group = 'org.example'
version = '1.0-SNAPSHOT'

repositories {
    mavenCentral()     // where dependencies are downloaded from
}

dependencies {
    implementation 'com.google.guava:guava:32.1.2-jre' // example dependency
    testImplementation 'org.junit.jupiter:junit-jupiter:5.10.0'
}

application {
    mainClass = 'org.example.App' // set your main class
}

test {
    useJUnitPlatform()
}
```

⚙️ This script tells Gradle:

* Which plugins to use (`java`, `application`)
* Where to get dependencies (`mavenCentral()`)
* What dependencies your app needs
* What the main class is for running the application

---

## 🧪 6. Add Your Java Code

In `src/main/java/org/example/App.java`:

```java
package org.example;

public class App {
    public static void main(String[] args) {
        System.out.println("Hello, Gradle!");
    }
}
```

And an example test in `src/test/java/org/example/AppTest.java`:

```java
package org.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class AppTest {
    @Test
    void testApp() {
        assertTrue(true);
    }
}
```

---

## 🚀 7. Common Gradle Commands

Use the **Gradle Wrapper** (`gradlew`) so everyone builds with the same Gradle version:

### Build the project

```bash
./gradlew build
```

This will compile code, run tests, and package the JAR. ([Medium][5])

### Run your application

```bash
./gradlew run
```

### Clean build files

```bash
./gradlew clean
```

---

## 📦 8. Create an Executable JAR

To specify the main class and build an executable JAR, add to `build.gradle`:

```groovy
jar {
    manifest {
        attributes 'Main-Class': 'org.example.App'
    }
}
```

Then build:

```bash
./gradlew jar
```

The JAR will be in:

```
build/libs/
```

Run it:

```bash
java -jar build/libs/MyGradleApp-1.0-SNAPSHOT.jar
```

---

## 📚 9. Learn More

* Official Gradle **Getting Started** walkthrough — from init to advanced tasks. ([docs.gradle.org][1])
* “Building Java Applications Sample” — step-by-step project example. ([docs.gradle.org][3])
* Medium / community tutorials for beginners and beyond. ([Medium][4])

---

If you want, I can tailor this with **IntelliJ IDEA examples**, **Kotlin DSL (`build.gradle.kts`)**, or **multi-module build tips** next — just let me know!

[1]: https://docs.gradle.org/current/userguide/getting_started_eng.html?utm_source=chatgpt.com "Getting Started"
[2]: https://gradle.org/install/?utm_source=chatgpt.com "Gradle | Installation"
[3]: https://docs.gradle.org/current/samples/sample_building_java_applications.html?utm_source=chatgpt.com "Building Java Applications Sample"
[4]: https://medium.com/%40AlexanderObregon/how-to-set-up-a-java-project-with-gradle-for-beginners-d425f61e457b?utm_source=chatgpt.com "How to Set Up a Java Project with Gradle for Beginners"
[5]: https://medium.com/%40vaasubisht/getting-started-with-gradle-building-and-running-java-applications-7c5e05eb60e3?utm_source=chatgpt.com "Getting Started with Gradle: Building and Running Java ..."
