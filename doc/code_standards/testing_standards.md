# Code Standards for Testing

## General Testing Principles
- Extensive unit tests
- A unit test function should test only one thing
- One assert per unit test function
- Use AssertJ for assertions instead of JUnit's built-in assertions
  - AssertJ provides more readable and descriptive test failures
  - See examples in the AssertJ Usage section below
- A unit test should be blazingly fast — maximum 10 milliseconds
  - Some exceptions can be made for tests involving time or similar behavior
- Test methods must be able to run in parallel
- Ensure edge cases are covered (null values, empty collections, boundary conditions)
- Be careful to test only what you intend to test — in other words, test as little as possible
  - For example: if you want to test an API method, move the logic to a separate function or class, so the logic can be tested without involving the web server
- Unit test classes do not have to declare "The purpose of this class..."

## Naming Conventions

### Classes
- Test classes are named: `[ClassToTest]Test`
- For example, for the class `Crypto`, the test class is named `CryptoTest`
- A utility class used in tests, located in the test package, is named: `[Something]Util`

### Functions
Test method names should follow this pattern:
```
unitOfWork_StateUnderTest_ExpectedBehavior
```

If longer descriptions or non-ASCII characters are needed:
```
`unit of work _ state under test _ expected behavior`
```

## Nested Test Classes
With JUnit 5, you can use nested classes to organize tests. If you have many tests for one unit-of-work, you can group them in a nested class:

```kotlin
@Nested
inner class UnitOfWork {
    @Test
    fun stateUnderTest1_ExpectedBehavior1{
        [...]
    }
    
    @Test
    fun stateUnderTest2_ExpectedBehavior2{
        [...]
    }
}
```

## Interface Testing
Every public interface or abstract class defining a significant unit of behavior must have a comprehensive contract test suite that rigorously verifies adherence to its documented purpose and constraints, independent of any specific implementation.

## AssertJ Usage

### Overview
AssertJ provides fluent, readable assertions that make test failures more descriptive and improve test readability.

### Key Points
- Always use static imports for AssertJ's `assertThat` method: `import static org.assertj.core.api.Assertions.assertThat`
- Prefer AssertJ's fluent API over JUnit's built-in assertions
- Take advantage of AssertJ's descriptive failure messages

### Examples

Instead of:
```kotlin
// JUnit style
assertEquals(3, parts.size)
assertEquals(1, parts[0].length)
assertTrue(parts[2].endsWith(".jpeg"))
```

Use:
```kotlin
// AssertJ style
assertThat(parts).hasSize(3)
assertThat(parts[0]).hasSize(1)
assertThat(parts[2]).endsWith(".jpeg")
```

### String Assertions
```kotlin
// Checking string content
assertThat("hello").isEqualTo("hello")
assertThat("hello").contains("el")
assertThat("hello").startsWith("he")
assertThat("hello").endsWith("lo")
assertThat("hello").hasSize(5)
```

### Collection Assertions
```kotlin
// For lists, sets, maps
assertThat(list).hasSize(3)
assertThat(list).contains("item")
assertThat(list).containsExactly("a", "b", "c")
assertThat(list).containsExactlyInAnyOrder("c", "a", "b")
assertThat(list).doesNotContain("item")
assertThat(map).containsKey("key")
```

### Numeric Assertions
```kotlin
// For integers, longs, doubles, etc.
assertThat(42).isEqualTo(42)
assertThat(42).isGreaterThan(41)
assertThat(42).isLessThan(43)
assertThat(42).isBetween(40, 45)
```

### Boolean Assertions
```kotlin
// For booleans
assertThat(true).isTrue
assertThat(false).isFalse
```

### Exception Assertions
```kotlin
// For exceptions
assertThatThrownBy { 
    // code that should throw an exception
}.isInstanceOf(IllegalArgumentException::class.java)
  .hasMessageContaining("specific message")
```

For more examples and advanced usage, refer to the [AssertJ documentation](https://assertj.github.io/doc/).
