# Code Standards
This is the code standards for humans and AI. 

## Classes
A class shall have:
- ONE purpose and handle ONE well-defined task
- Preferably less than 100 lines 
- Max 250 lines including comments and blank lines
- Max 5 public functions

## Functions
A function:
- Performs ONE well defined task
- Has 10 lines of code or less, excluding comments

## In Code Documentation
- All classes, interfaces, files start with "The purpose of this [class/interface/...] is to [...]"
  - This helps create a concise explanation of what a class does
  - It forces an explanation of why, rather than what
- **Comment placement**: Write comments ABOVE the line of code, never after (except for rare cases with short repetitive declarations where inline makes more sense)
- **Comment density**: Aim for one comment line per line of meaningful code
- **Explain WHY not WHAT**: Focus on business logic, decisions, and non-obvious implementations
- Document so that when you return six months later you are happy with yourself
- We use US English

Example:
```kotlin
// Calculate daily drift from annual trend strength
val dailyDrift = trend.strength / 100.0 / 252.0
// Apply geometric Brownian motion with market trend influence  
val newPrice = currentPrice * exp(dailyDrift + volatility * randomShock)
```

## Line length
Most lines should be 80 chars or shorter.
81-120 chars in rare circumstances. 
One should have a very good reason to go above 120 chars.

## Naming and Casing Conventions

| Type       | Convention                  | Example                                   |
|------------|-----------------------------|-------------------------------------------|
| Class      | PascalCase                  | `MyLogger`                                |
| Interface  | PascalCase with `I` prefix  | `IFileStorage`                            |
| Function   | camelCase                   | `registerUser()`                          |
| Variable   | camelCase                   | `recipeName`                              |
| Constant   | UPPER_SNAKE_CASE            | `MAX_RETRIES`                             |
| Package    | snake_case                  | `my_project.sites.admin.pages.admin_user` |
| Enum Class | PascalCase with suffix enum | `UnitEnum`                                |
| Enum Value | UPPER_SNAKE_CASE            | `TABLESPOON`                              |
| File Name  | PascalCase for classes      | `MyLogger.kt`                             |
| Directory  | lowercase with underscores  | `file_storage`                            |

### Numbers
For readability use thousand-separator for numerical values 10 000 and larger. 
That is use 129_690_289 instead of 129690289. 
This does not apply to ids. 
This applies to numerical data types and not strings.

## Code
### Interfaces
Extensively use interfaces to define clear contracts and decouple components. Explicit interface contracts with strong typing and validation requirements.
- Since AI might implement functions differently than expected, well-defined interfaces with clear input/output requirements ensure architectural boundaries remain intact regardless of implementation details.
- Every public interface or abstract class defining a significant unit of behavior must have a comprehensive contract test suite that rigorously verifies adherence to its documented purpose and constraints, independent of any specific implementation.

Example:
```kotlin
/**
 * The purpose of this interface is to define the contract for
 * handling user registration with validation requirements.
 *
 * Input validation happens at this boundary before any business
 * logic is executed.
 */
interface UserRegistrationPort {
    /**
     * Registers a new user after validating input
     * @param request Must contain non-empty email and password (8+ chars)
     * @return Success with userId or failure with error details
     * @throws ValidationException if request doesn't meet requirements
     */
    fun registerUser(request: RegistrationRequestDTO): Result<UserId>
}
```

### Immutability
Favor immutability for better code predictability:
- Use immutable collection like Kotlin's immutable collections, Java's Collections.unmodifiable*, or libraries like Guava/Vavr
- Avoid mutable variables (var in Kotlin, non-final fields/variables in Java)
- Avoid side effects within functions and classes

### Defensive Programming
Use defensive programming:
- Fail fast: Detect problems early, as close to the source as possible
- Prefer exceptions over nulls: Functions should throw exceptions rather than return null/optional types when data isn't found
- Validate inputs: Never assume arguments or data are valid — check them
- Check assumptions: If your code assumes a condition is true (e.g., a dependency is initialized), make that assumption explicit and enforced
- Handle edge cases: Nulls, empty lists, timeouts, division by zero, etc.
- Isolate failures: Don't let one bad input or bug take down the whole system
- Add guardrails: For example, require(), check(), assert(), or fallback logic

### Type Design
Always use meaningful domain types instead of generic container types like `Pair`, `Triple`, or `Map<String, Any>`.

**✅ Correct:**
```kotlin
data class CustomerLifecycle(
    val joinDate: LocalDate,
    val departureDate: LocalDate,
    val status: String
)

fun generateCustomerLifecycle(): CustomerLifecycle
```

**❌ Avoid:**
```kotlin
fun generateCustomerLifecycle(): Triple<LocalDate, LocalDate, String>
fun getUserInfo(): Pair<String, Int>
```

Benefits: Self-documenting code, type safety, better maintainability, and meaningful IDE auto-completion.

### Avoid Cleverness
- The cleverer the code, the harder it is to understand and maintain
- Prioritize clarity over elegance unless the solution can be easily explained
- Write code that others (including your future self) can quickly comprehend

## Naming
- Variables, classes, functions and files should have self-explanatory names
  - The aim is that the naming is so good that comments are superfluous
- Data classes - suffix with Dto
- Objects that are stored in the database - suffix with Dbo
- Data access classes or objects - suffix with Dao
