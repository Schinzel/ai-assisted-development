# Code Standards for AIs

This is a code standard to be followed by AIs such as Claude, Gemini, ChatGPT and so on.

## Identification
- If creating a class or function, add as last line in the header:
  ```kotlin
  Written by [Model] with [version of Code Standard]
  ```
  For example:
  ```kotlin
  Written by Claude 3.7 with Code Standard 1.2
  ```

## General Guidelines
- All functions should be Enterprise Production Ready for live deployment. 
- Check all of the code for bugs and implement fixes, ensure all potential edge cases are handled.
- Everything should be held to the highest standards for performance, it should be blazing fast. 
- Ensure maximum robustness. 
- Try catch and finally everywhere appropriate.
- Break code into the smallest possible, independently testable units. This is usually true, but this is extra important for AIs.
- Extensively use interfaces to define clear contracts and decouple components.
- Since AI might implement functions differently than expected, well-defined interfaces with clear input/output requirements ensure architectural boundaries remain intact regardless of implementation details.

## Interface Design
Every public interface or abstract class defining a significant unit of behavior must have a comprehensive contract test suite that rigorously verifies adherence to its documented purpose and constraints, independent of any specific implementation.

Example:
```kotlin
/**
 * The purpose of this interface is to define the contract for
 * handling user registration with validation requirements.
 *
 * Input validation happens at this boundary before any business
 * logic is executed.
 *
 * Written by Claude 3.7 with Code Standard 1.2
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

## AI-specific Considerations
- **Immutability**: AIs should favor immutability
  - Use immutable collection like Kotlin's immutable collections, Java's Collections.unmodifiable*, or libraries like Guava/Vavr
  - Avoid mutable variables (var in Kotlin, non-final fields/variables in Java)
  - Avoid side effects within functions and classes

- **Avoid Cleverness**:
  - The cleverer the code, the less likely the AI is to understand it on first pass
  - Go for clarity over elegance unless you can explain it easily in a prompt
