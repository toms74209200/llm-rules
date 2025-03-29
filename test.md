# How to Write Tests

## Testability
- Do not use mocks
- Do not use hidden inputs; pass all inputs as function arguments
- Avoid hidden outputs where possible; return all outputs as function return values
- Functions should be stateless and have referential transparency; pass state as arguments
- For asynchronous operations, use Feature/Promise or Observer pattern, or pass callback functions as arguments
- For returning values and recoverable errors, use language API's built-in tuple or Either/Result types. Do not create custom error types
- Functions should not throw unchecked exceptions

## Test Types and Size Classification
Refer to Google Testing Blog https://testing.googleblog.com/2010/12/test-sizes.html

**Important Principle**: All unit tests must be small tests, and acceptance tests should be medium or large tests depending on implementation requirements

### Test Size Constraints:
    | Feature               | Small               | Medium                    | Large                        |
    |----------------------|---------------------|--------------------------|------------------------------|
    | Time limit (seconds) | 60                  | 300                      | 900+                         |
    | Network              | Not allowed         | localhost only           | Allowed                      |
    | Database             | Not allowed         | Allowed                  | Allowed                      |
    | File system access   | Not allowed         | Not recommended          | Allowed                      |
    | Multi-threading      | Not allowed         | Allowed                  | Allowed                      |
    | Sleep statements     | Not allowed         | Allowed                  | Allowed                      |
    | System properties    | Not allowed         | Allowed                  | Allowed                      |

## Acceptance Test Process
- Implement acceptance tests using Behavior Driven Development (BDD)
- Write acceptance test specifications in Gherkin format feature files
- Place feature files in the `/features` directory with `.feature` extension
- Implement acceptance tests with appropriate test size (medium or large)
- Choose test size based on scenario resource requirements
