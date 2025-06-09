# Phase 4 - User pipeline

# Current Implementation Status (to be completed by AI)
Task 1 done - User Generator - 2025-06-06
Task 2 done - Transaction Generator - 2025-06-06

# Tasks

## Phase 4 - Task 1 - User Generator → `users.txt`
Create UserDataGenerator that implements IDataGenerator to generate user data.

### Description
Implement a generator that creates realistic user data with proper distribution of investor profiles, activity levels, and customer countries. Includes customer lifecycle management with join/departure dates and customer status tracking.

### Uses
- InvestorProfileCollection
- ActivityLevelCollection
- CustomerCountriesCollection

### Parameters
- Number of users
- customerJoinDistribution (e.g., 30% join after simulation start date)
- customerDepartureRate (e.g., 20% leave before simulation end date)
- simulationStartDate and simulationEndDate for determining join/departure date ranges

### Output
- user_id
- investor_profile_id
- activity_level_id
- country_id
- join_date (when customer became active)
- departure_date (when customer left, null if still active)
- customer_status (ACTIVE, DEPARTED)

### Customer Lifecycle Logic
- 30% of users have join_date after simulationStartDate (spread across entire simulation range)
- 20% of users have departure_date before simulationEndDate with customer_status = DEPARTED
- Remaining users start at simulationStartDate and remain ACTIVE throughout
- Departed customers trigger instant sell-off transactions in Transaction Generator

### Deliverables
- UserDataGenerator class
- Integration with existing data collections

### Acceptance Criteria
1. Implements IDataGenerator interface
2. Generates users distributed according to investor profile, activity level, and customer country percentages
3. Implements customer lifecycle logic with join/departure dates and status tracking
4. Handles customerJoinDistribution and customerDepartureRate parameters
5. Creates realistic user demographics with proper date ranges

### Task Summary (to be completed by AI)
**Phase 4 - Task 1 - User Generator Complete ✅**

**Major Changes Made:**
- Created `UserDataGenerator` class implementing `IDataGenerator` interface for generating realistic user data
- Added `CustomerLifecycle` data class to encapsulate customer lifecycle information with clear property names
- Added `CustomerStatus` enum for type-safe customer status representation (ACTIVE, DEPARTED)

**Files Created/Modified:**
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/user_data_generator/UserDataGenerator.kt` (new)
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/user_data_generator/CustomerLifecycle.kt` (new)
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/user_data_generator/CustomerStatus.kt` (new)
- `src/test/kotlin/com/wisecrowd/data_generator/data_generators/user_data_generator/UserDataGeneratorTest.kt` (new)

**Key Implementation Decisions:**
- Uses weighted random selection from existing data collections (InvestorProfile, ActivityLevel, CustomerCountries)
- Implements customer lifecycle with configurable join/departure distribution parameters (30%/20% defaults)
- Uses sentinel date (9999-12-31) for users who never departed instead of null values for data consistency
- Generates UUID for user_id to ensure uniqueness across large datasets
- Implements defensive programming with comprehensive input validation

**Technical Approach:**
- Customer lifecycle logic: 30% join after simulation start, 20% depart before end, remaining stay active throughout
- Departed customers have real departure dates > join dates, active customers use sentinel date
- Uses `WeightedRandomSelector` utility for proper distribution according to collection percentages

**Items Affecting Future Tasks:**
- `CustomerStatus.DEPARTED` users with real departure dates will need special handling in Transaction Generator for sell-off logic
- Generated user data structure matches expected schema for Transaction and Holdings generators
- Sentinel date pattern established for handling "never occurred" dates consistently across system

## Phase 4 - Task 2 - Transaction Generator → `transactions.txt`
Create TransactionDataGenerator that implements IDataGenerator to generate transaction data.

### Description
Implement a generator that creates realistic transaction data with proper currency distribution and user activity patterns. Includes special handling for departed customers who trigger instant sell-off transactions.

### Uses
- Price series data
- User data  
- CurrencyCollection

### Customer Lifecycle Integration
- Process users with DEPARTED status to generate sell-off transactions
- Departed customers sell all holdings on their departure_date
- Regular ACTIVE customers follow normal activity patterns based on their activity_level

### Deliverables
- TransactionDataGenerator class
- Integration with price series, users, and currencies

### Acceptance Criteria
1. Implements IDataGenerator interface
2. Generates transactions distributed according to currency and user activity patterns
3. Handles departed customer sell-off logic using customer_status and departure_date
4. Creates realistic transaction amounts, dates, and types based on price series
5. Processes both ACTIVE and DEPARTED customer scenarios

### Task Summary (to be completed by AI)
**Phase 4 - Task 2 - Transaction Generator Complete ✅**

**Major Changes Made:**
- Created `TransactionDataGenerator` class implementing `IDataGenerator` interface for generating realistic transaction data
- Implemented separate `TransactionGenerator` class for clean separation of transaction generation logic 
- Added comprehensive data classes: `PriceData`, `AssetPriceCollection`, `TransactionRequest`, and `UserData` for type safety
- Integrated customer lifecycle logic with proper handling of ACTIVE and DEPARTED customer scenarios

**Files Created/Modified:**
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/TransactionDataGenerator.kt` (new)
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/transaction_data_generator/TransactionGenerator.kt` (new)
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/transaction_data_generator/PriceData.kt` (new)
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/transaction_data_generator/AssetPriceCollection.kt` (new)
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/transaction_data_generator/TransactionRequest.kt` (new)
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/transaction_data_generator/UserData.kt` (new)
- `src/test/kotlin/com/wisecrowd/data_generator/data_generators/TransactionDataGeneratorTest.kt` (new)
- `src/test/kotlin/com/wisecrowd/data_generator/data_generators/transaction_data_generator/PriceDataTest.kt` (new)
- `src/test/kotlin/com/wisecrowd/data_generator/data_generators/transaction_data_generator/AssetPriceCollectionTest.kt` (new)

**Key Implementation Decisions:**
- Separated transaction generation logic into dedicated `TransactionGenerator` class following single responsibility principle
- Replaced complex `Map<UUID, List<Triple<...>>>` structures with clean, readable data classes
- Used pure functions that return data instead of modifying class scope variables for better maintainability
- Implemented activity level-based transaction frequency (1-75 transactions/year based on activity level)
- Currency distribution follows design specification percentages using `WeightedRandomSelector`

**Technical Approach:**
- DEPARTED customers generate sell-off transactions on departure date (2-5 assets, larger amounts)
- ACTIVE customers generate regular buy/sell transactions based on activity level throughout their active period
- Transaction amounts scaled by asset price to ensure realistic share quantities (1,000-50,000 SEK range)
- Defensive programming with comprehensive input validation and error handling

**Items Affecting Future Tasks:**
- Transaction data structure established for `UserHoldingsDataGenerator` input processing
- Asset price collection pattern available for reuse in other components requiring price data access
- Customer lifecycle integration demonstrates pattern for handling user status changes across generators

## Phase 4 - Task 3 - User Holdings Generator → `user_holdings.txt`
Create UserHoldingsDataGenerator that implements IDataGenerator to generate user holdings data.

### Description
Implement a generator that calculates current user portfolio positions by processing all transactions and computing net holdings for each user-asset combination.

### Uses
- Transaction data

### Deliverables
- UserHoldingsDataGenerator class
- Integration with transaction data

### Acceptance Criteria
1. Implements IDataGenerator interface
2. Takes transaction data as input and calculates net positions
3. Aggregates buy/sell transactions to determine current holdings
4. Handles multiple currencies properly
5. Generates realistic user_holdings.txt output

### Task Summary (to be completed by AI)
**Phase 4 - Task 3 - User Holdings Generator Complete ✅**

**Major Changes Made:**
- Created `UserHoldingsDataGenerator` class implementing `IDataGenerator` interface for calculating user portfolio holdings from transaction data
- Implemented transaction aggregation logic to calculate net positions for each user-asset-currency combination
- Added comprehensive input validation with defensive programming principles

**Files Created/Modified:**
- `src/main/kotlin/com/wisecrowd/data_generator/data_generators/UserHoldingsDataGenerator.kt` (new)
- `src/test/kotlin/com/wisecrowd/data_generator/data_generators/UserHoldingsDataGeneratorTest.kt` (new)

**Key Implementation Decisions:**
- Uses `HoldingKey` data class for clean aggregation by user-asset-currency combination instead of complex map structures
- Processes transactions chronologically to maintain running balances and calculate net positions
- Excludes zero and negative holdings from output (users cannot have negative asset positions)
- Handles multiple currencies separately for same user-asset combinations
- Pre-calculates all holdings during initialization for consistent iteration behavior

**Technical Approach:**
- Aggregates buy transactions (positive amounts) and sell transactions (negative amounts) for each unique holding position
- Only outputs positive net holdings representing actual portfolio positions
- Uses defensive programming with comprehensive input validation for transaction data structure
- Follows existing code standards with proper documentation and type safety

**Items Affecting Future Tasks:**
- Completes the user holdings pipeline for the WiseCrowd data generation system
- Generated holdings data represents final portfolio positions after all transactions processed
- Holdings structure ready for integration with reporting or analysis components
