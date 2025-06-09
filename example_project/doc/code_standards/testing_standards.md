# Code Standards for Testing

## General Testing Principles
- Extensive unit tests
- A unit test function should test only one thing
- One assertThat per unit test function
- Use AssertJ for assertions instead of JUnit's built-in assertions
  - AssertJ provides more readable and descriptive test failures
  - See examples in the AssertJ Usage section below
- A unit test should be blazingly fast — maximum 10 milliseconds
  - Some exceptions can be made for tests involving time or similar behavior
- Test methods must be able to run in parallel
- Ensure edge cases are covered (null values, empty collections, boundary conditions)
- Be careful to test only what you intend to test — in other words, test as little as possible
  - For example: if you want to test an API method, move the logic to a separate function or class, so the logic can be tested without involving the web server
- Single point of truth for testing - Each behavior should be tested in exactly one place
- Unit test classes do not have to declare "The purpose of this class..."

## Naming Conventions

### Classes
- Test classes are named: `[ClassToTest]Test`
- For example, for the class `Crypto`, the test class is named `CryptoTest`
- A utility class used in tests, located in the test package, is named: `[Something]Util`

### Functions
Test method names **MUST** follow this exact pattern:
```
unitOfWork_StateUnderTest_ExpectedBehavior
```

If longer descriptions or non-ASCII characters are needed:
```
`unit of work _ state under test _ expected behavior`
```

If the unit of work tested is the constructor, write that explicitly.
For example:
```
`constructor _ valid asset class _ creates object with correct id`
```

### Function Naming Examples (Required for Claude Code)

**✅ Correct Examples:**
```kotlin
@Test
fun constructor_zeroAssetCount_throwsIllegalArgumentException()

@Test
fun getColumnNames_defaultConfiguration_returnsCorrectColumnNames()

@Test
fun `has more rows _ all assets generated _ returns false`()
```

**Naming Components:**
- **unitOfWork**: Exact method name (`getColumnNames`, `hasMoreRows`) or `constructor`
- **StateUnderTest**: Input scenario (`defaultConfiguration`, `zeroAssetCount`, `validAssetClass`)
- **ExpectedBehavior**: Expected outcome (`returnsCorrectColumnNames`, `throwsIllegalArgumentException`, `returnsTrue`)


## Nested Test Classes
With JUnit 5, you can use nested classes to organize tests. If you have many tests for one unit-of-work, you can group them in a nested class:

```kotlin
@Nested
inner class UnitOfWork {
    @Test
    fun stateUnderTest1_ExpectedBehavior1(){
        //...
    }
    
    @Test
    fun stateUnderTest2_ExpectedBehavior2(){
        //...
    }
}
```
or

```kotlin
@Nested
inner class UnitOfWork {
    @Test
    fun `state under test _ expected behavior`(){
        //...
    }   
}
```

## AssertJ Usage
Use AssertJ for more readable assertions and better error messages.

**Examples:**
```kotlin
// Instead of JUnit assertEquals/assertTrue
assertThat(parts).hasSize(3)
assertThat(parts[0]).hasSize(1) 
assertThat(parts[2]).endsWith(".jpeg")
assertThat(true).isTrue
assertThat(list).contains("item")
assertThat(42).isGreaterThan(41)

// Exception testing
assertThatThrownBy { 
    // code that throws
}.isInstanceOf(IllegalArgumentException::class.java)
```

## Minimal assertion logic
In the asserts there should be minimal logic. The only thing that should be able to fail 
in a test is the test. So all logic outside of the assert. 

That is, I want 
```kotlin
val expectedDate = LocalDate.of(2020, 3, 1)
assertThat(marketTrend.startDate).isEqualTo(expectedDate)
```
and not
```kotlin
assertThat(marketTrend.startDate).isEqualTo(LocalDate.of(2020, 3, 1))
```

That is, I want
```kotlin
val name = nordicStocks?.name
assertThat(name).isEqualTo("Nordic stocks")
```
and not
```kotlin
assertThat(nordicStocks?.name).isEqualTo("Nordic stocks")
```

