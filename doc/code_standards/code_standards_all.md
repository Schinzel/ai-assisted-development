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
| Variable   | snake_case                  | `recipe_name`                             |
| Constant   | UPPER_SNAKE_CASE            | `MAX_RETRIES`                             |
| Package    | snake_case                  | `my_project.sites.admin.pages.admin_user` |
| Enum Class | PascalCase with suffix enum | `UnitEnum`                                |
| Enum Value | UPPER_SNAKE_CASE            | `TABLESPOON`                              |
| File Name  | PascalCase for classes      | `MyLogger.kt`                             |
| Directory  | lowercase with underscores  | `file_storage`                            |

### Numbers
For readability use thousand-separator for numerical values 10 000 and larger. 
That is user 129_690_289 instead of 129690289. 
This does not apply to ids. 
This applies to numerical data types and not strings.

## Code
### Interfaces
Extensively use interfaces to define clear contracts and decouple components. Explicit interface contracts with strong typing and validation requirements. 🔒
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
Favor Immutability (for the sake of AIs):
- Use immutable collection like Kotlin's immutable collections, Java's Collections.unmodifiable*, or libraries like Guava/Vavr
- Avoid mutable variables (var in Kotlin, non-final fields/variables in Java)
- Avoid side effects within functions and classes

### Defensive Programming
Use defensive programming:
- Fail fast: Detect problems early, as close to the source as possible
- Validate inputs: Never assume arguments or data are valid — check them
- Check assumptions: If your code assumes a condition is true (e.g., a dependency is initialized), make that assumption explicit and enforced
- Handle edge cases: Nulls, empty lists, timeouts, division by zero, etc.
- Isolate failures: Don't let one bad input or bug take down the whole system
- Add guardrails: For example, require(), check(), assert(), or fallback logic

### Avoid Cleverness
- The cleverer the code, the less likely the AI is to understand it on first pass
- Go for clarity over elegance unless you can explain it easily in a prompt

## Naming
- Variables, classes, functions and files should have self-explanatory names
  - The aim is that the naming is so good that comments are superfluous
- Data classes - suffix with Dto
- Objects that are stored in the database - suffix with Dbo
- Data access classes or objects - suffix with Dao

## Documentation
- All classes, interfaces, files and so on start with "The purpose of this [class/interface/...] is to [...]"
  - This helps create a concise explanation of what a class does
  - It forces an explanation of why, rather than what
- One line of code = (roughly) one line of documentation
- Document so that when you return to the documentation six months from now that you are happy with yourself
- Documentation outside the code should be to the point and maintainable
- We use US English
