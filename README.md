# Kotlin Power-Assert Plugin Example

This project demonstrates the [Kotlin Power-Assert compiler plugin](https://kotlinlang.org/docs/power-assert.html), which transforms assertion expressions to provide detailed diagnostic information when assertions fail.

## What is Power-Assert?

Power-Assert is a compiler plugin that enhances assertion failures with visual diagrams showing:
- The values of all sub-expressions at the time of failure
- The evaluation tree of the expression
- Property accesses, method calls, and their results

### Example Output

When an assertion fails, instead of just seeing "Assertion failed", you get a detailed breakdown:

```
[ERROR]   AppTest.testFunction:10 Incorrect length
assert(hello.length == world.substring(1, 4).length) { "Incorrect length" }
       |     |      |  |     |               |
       |     |      |  |     |               3
       |     |      |  |     orl
       |     |      |  world!
       |     |      false
       |     5
       Hello
```

This makes debugging much easier by showing exactly what values were present when the assertion failed.

## Configuration

This project includes configuration for both **Gradle** and **Maven** build systems.

### Gradle Configuration

In `build.gradle.kts`:

```kotlin
plugins {
    kotlin("plugin.power-assert") version "2.2.21"
}

powerAssert {
    functions = listOf(
        "kotlin.assert",
        "kotlin.test.assertTrue",
        "kotlin.test.assertEquals"
    )
}
```

### Maven Configuration

In `pom.xml`:

```xml
<plugin>
    <groupId>org.jetbrains.kotlin</groupId>
    <artifactId>kotlin-maven-plugin</artifactId>
    <version>${kotlin.version}</version>
    <configuration>
        <compilerPlugins>
            <plugin>power-assert</plugin>
        </compilerPlugins>
        <pluginOptions>
            <option>power-assert:function=kotlin.assert</option>
            <option>power-assert:function=kotlin.test.assertTrue</option>
            <option>power-assert:function=kotlin.test.AssertEquals</option>
        </pluginOptions>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-power-assert</artifactId>
            <version>2.2.21</version>
        </dependency>
    </dependencies>
</plugin>
```

## Running Tests

### With Gradle
```bash
./gradlew test
```

### With Maven
```bash
mvn test
```

### Building and Running

#### Gradle
```bash
./gradlew build
./gradlew run
```

#### Maven
```bash
# Build the project
mvn -q -e -DskipTests package

# Run the application
java -jar target/kotlin-junit-complete-1.0-SNAPSHOT.jar
```

## Requirements

- Java 17+ (Maven) or Java 21+ (Gradle)
- Kotlin 2.2.21+

## How It Works

The Power-Assert plugin transforms assertion functions at compile time. When you write:

```kotlin
assert(hello.length == world.substring(1, 4).length)
```

The compiler automatically instruments the code to capture intermediate values. If the assertion fails, it reconstructs the evaluation tree and displays it in a human-readable format.

## Supported Functions

You can configure which functions should be transformed. This project includes:
- `kotlin.assert`
- `kotlin.test.assertTrue`
- `kotlin.test.assertEquals`

You can add your own custom assertion functions to the configuration.

## Project Structure

```
src/
├── main/kotlin/    — Application sources
└── test/kotlin/    — Tests using kotlin.test on JUnit 5
```

## Reference Documentation

* [Kotlin Power-Assert Documentation](https://kotlinlang.org/docs/power-assert.html)
* [Official Gradle documentation](https://docs.gradle.org)
* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
* [Kotlin Documentation](https://kotlinlang.org/docs/home.html)
