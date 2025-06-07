# Phase 1 - Data collections & Data Saver

# Current Implementation Status (to be completed by AI)
- Task 0 done - Created FileNameEnum to store output file names in one place
- Task 1 done - Created FileDataSaver to save data to text files with proper formatting and error handling
- Task 2 done - Created MarketTrend and MarketTrendCollection classes to manage market trend data
- Task 3 done - Created AssetClass and AssetClassCollection classes to manage asset class data
- Task 4 done 2025-05-28 - Moved asset_class and market_trend packages to data_collections package
- Task 5 done 2025-05-28 - Created Currency and CurrencyCollection classes to manage currency data
- Task 6 done 2025-05-28 - Created InvestorProfile and InvestorProfileCollection classes to manage investor profile data
- Task 7 done 2025-05-28 - Created ActivityLevel and ActivityLevelCollection classes to manage activity level data

# Tasks

## Task 0 - File names in an enum
The names of the resulting files should be in an enum so that they are
in one place. The enum should be in the data_generator package. 

### Task Summary (to be completed by AI)
- **Changes Made**: Created a new enum class `FileNameEnum` in the `com.wisecrowd.data_generator` package to store all output file names in one centralized location.

- **Files Created**: 
  - `/src/main/kotlin/com/wisecrowd/data_generator/FileNameEnum.kt`

- **Implementation Details**:
  - Implemented an enum with four entries corresponding to the required output files: ASSET_DATA, USERS, PRICE_SERIES, and TRANSACTIONS
  - Each enum constant has a `fileName` property with the corresponding file name string (e.g., "asset_data.txt")
  - Added proper documentation including the purpose of the enum and the Claude signature as required by the code standards
  - Used clear naming conventions according to the project's code standards

- **Verification**: Successfully built and tested the project to ensure the changes didn't break any existing functionality.


## Task 1 - File Data Save
Based on IDataSaver implement a FileDataSaver. 
- The FileDataSaver should:
  - Save data to the specified file path
  - Use "\n" as row delimiter (stored as a constant)
  - Use "\t" as column delimiter (stored as a constant)
  - Use "###" as string qualifier (stored as a constant)
  - Include a constructor parameter for the file path
  - Create the output directory if it doesn't exist
  - Create a header row based on the column names in the prepare() method
  - Use defensive programming principles (validate inputs, handle exceptions properly)
  - Close file resources properly in the complete() method

### Acceptance Criteria
1. FileDataSaver correctly writes data to files with proper delimiters
2. All errors during the save process are captured and available via getErrors()
3. The class includes proper resource management (files are closed after use)
4. Unit tests cover normal operation and error cases
5. Code follows the project's code standards
6. The implementation can handle all data types defined in DataTypeEnum
7. Unit tests test edge cases such as cyrillic and polish chars, long and short strings. Use the Class io.schinzel.basicutils.FunnyChars

### Task Summary (to be completed by AI)
- **Changes Made**: Implemented a new `FileDataSaver` class that fulfills the IDataSaver interface to write data to files with proper formatting and error handling.

- **Files Created**: 
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_saver/FileDataSaver.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_saver/FileDataSaverTest.kt`

- **Implementation Details**:
  - Created a class that saves data to a specified file path with:
    - "\n" as row delimiter (stored as a constant ROW_DELIMITER)
    - "\t" as column delimiter (stored as a constant COLUMN_DELIMITER)
    - "###" as string qualifier (stored as a constant STRING_QUALIFIER)
  - Implemented constructor to create parent directories if they don't exist
  - Implemented prepare() method to create the file and header row
  - Implemented saveItem() to save data with proper formatting based on data type
  - Implemented complete() method to properly close resources
  - Implemented error handling with getErrors() and hasErrors() methods
  - Used defensive programming principles throughout:
    - Validation of inputs (empty column data, data/column size mismatch)
    - Proper exception handling with informative error messages
    - Proper resource management for file handling

- **Testing**:
  - Created comprehensive test suite covering all requirements and edge cases:
    - Testing file creation and directory structure creation
    - Testing header row generation
    - Testing data formatting for all data types
    - Testing special character handling (Cyrillic, Polish, etc.)
    - Testing long string handling
    - Testing empty string handling
    - Testing error conditions and proper error reporting
    - Testing proper resource cleanup

- **Verification**: All tests pass successfully, demonstrating the robustness of the implementation.

- **Future Considerations**: The FileDataSaver will be used by various data generators to save their output to the specified files.


## Task 2 - MarketTrend data
Generate a class that holds the market trend data. Create the class MarketTrend that holds
a row of data. The class that holds the rows is called MarketTrendCollection. 

### Task Summary (to be completed by AI)
- **Changes Made**: Implemented MarketTrend and MarketTrendCollection classes to represent and manage market trend data.

- **Files Created**: 
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/market_trend/MarketTrend.kt`
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/market_trend/MarketTrendCollection.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/market_trend/MarketTrendTest.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/market_trend/MarketTrendCollectionTest.kt`

- **Implementation Details**:
  - **MarketTrend class**: 
    - Implemented as a data class to represent a single market trend with startDate, endDate, trendType, strength, and description
    - Added validation in the init block to ensure end date is not before start date, strength is within a reasonable range, and trend type is not empty

  - **MarketTrendCollection class**:
    - Implemented methods to add, retrieve, and filter market trends
    - Created methods to filter trends by date (getTrendsOnDate) and date range (getTrendsInRange)
    - Added a companion object with a static createDefaultCollection() method that creates the predefined Swedish/Nordic market trends from 1990-2025 as specified in the design document

- **Testing**:
  - Created comprehensive tests for both classes
  - Tested validation rules for MarketTrend
  - Tested collection operations and filtering capabilities for MarketTrendCollection
  - Verified that the default collection contains the expected number of trends and that specific historical events are correctly represented

- **Verification**: Successfully built and tested the implementation, with all tests passing.

- **Future Considerations**: The MarketTrendCollection will be used by the Price Series Generator to create historical price data for assets based on market trends.

## Task 3 - Asset class data
Generate a class that holds the asset class data. Create the class AssetClass that holds
a row of data. AssetClassCollection holds the rows. 

### Task Summary (to be completed by AI)
- **Changes Made**: Implemented AssetClass and AssetClassCollection classes to represent and manage asset class data.

- **Files Created**: 
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/asset_class/AssetClass.kt`
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/asset_class/AssetClassCollection.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/asset_class/AssetClassTest.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/asset_class/AssetClassCollectionTest.kt`

- **Implementation Details**:
  - **AssetClass class**: 
    - Implemented as a data class to represent a single asset class with id, name, description, volatilityLevel, and prevalencePercentage
    - Added validation in the init block to ensure positive ID, non-empty name/description/volatilityLevel, and prevalence percentage within 0-100 range

  - **AssetClassCollection class**:
    - Implemented methods to add, retrieve, and filter asset classes
    - Created getById method that throws IllegalArgumentException if asset class not found (fail-fast approach)
    - Added filtering methods: getByVolatilityLevel (case-insensitive) and getByPrevalenceRange with proper validation
    - Added a companion object with createDefaultCollection() method that creates the predefined asset classes from the design document (8 asset classes including Nordic stocks, Government bonds, Corporate bonds, etc.)

- **Testing**:
  - Created comprehensive tests for both classes following code standards
  - Used proper test naming convention with method_condition_expectedResult format
  - Separated logic from assertions (extracted values before assertThat calls)
  - Tested validation rules, collection operations, filtering capabilities, and edge cases
  - Verified that the default collection contains the expected asset classes with correct properties

- **Verification**: Successfully built and tested the implementation with all 74 tests passing.

- **Future Considerations**: The AssetClassCollection will be used by the Asset Generator to create investment assets based on asset classes and their prevalence percentages.

## Task 4 - Move classes
I want all data classes gathered in one package. 
Move the packages `asset_class` and `market_trend` to a new package `data_collections`.
E.g. instead of `data_generator/asset_class` we will have
`data_generator/data_collections/asset_class`
- Move corresponding test classes
- Update file paths in `Task Summary (to be completed by AI)` for task 2 and 3.

### Task Summary (to be completed by AI)
- **Changes Made**: Successfully moved the `asset_class` and `market_trend` packages to a new `data_collections` package structure to better organize data classes.

- **Files Moved**:
  - From: `/src/main/kotlin/com/wisecrowd/data_generator/asset_class/` 
  - To: `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/asset_class/`
  - From: `/src/main/kotlin/com/wisecrowd/data_generator/market_trend/`
  - To: `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/market_trend/`
  - Also moved corresponding test files from `/src/test/kotlin/com/wisecrowd/data_generator/` to `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/`

- **Implementation Details**:
  - Updated package declarations in all moved files to reflect new package structure
  - Maintained all existing functionality and tests unchanged
  - Updated file paths in Task Summary sections for Task 2 and Task 3 to reflect new locations
  - Verified build and all tests still pass (74 tests successful)

- **Verification**: Successfully built and tested the project to ensure the package restructuring didn't break any existing functionality.

- **Future Considerations**: All future data collection classes should be placed under the `data_collections` package to maintain organization consistency.


## Task 5 - Currencies
Generate a class that holds the currency data. Create the class Currency that holds
a row of data. CurrencyCollection holds the rows.

### Task Summary (to be completed by AI)
- **Changes Made**: Implemented Currency and CurrencyCollection classes to represent and manage currency data.

- **Files Created**: 
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/currency/Currency.kt`
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/currency/CurrencyCollection.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/currency/CurrencyTest.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/currency/CurrencyCollectionTest.kt`

- **Implementation Details**:
  - **Currency class**: 
    - Implemented as a data class to represent a single currency with id, code, name, distributionPercentage, and conversionToSek
    - Added validation in the init block to ensure positive ID, non-empty code/name, distribution percentage within 0-100 range, and positive conversion rate to SEK

  - **CurrencyCollection class**:
    - Implemented methods to add, retrieve, and filter currencies
    - Created getById and getByCode methods that throw IllegalArgumentException when currency not found (following defensive programming and fail-fast principles)
    - Added getByDistributionRange method with proper validation for percentage ranges
    - Added a companion object with createDefaultCollection() method that creates the predefined currencies from the design document (8 currencies including SEK, EUR, USD, NOK, DKK, GBP, JPY, CHF)

- **Testing**:
  - Created comprehensive tests for both classes following the project's testing standards
  - Used one assertThat per test function and minimal logic in assertions as required
  - Tested validation rules, collection operations, filtering capabilities, and edge cases
  - Verified that the default collection contains the expected currencies with correct properties

- **Verification**: Successfully built and tested the implementation with all 122 tests passing, including 48 new tests for the currency classes.

- **Future Considerations**: The CurrencyCollection will be used by the Transaction Generator to handle different currency types and conversions in financial transactions.

## Task 6 - Investor Profiles
Generate a class that holds the investor profile data. Create the class InvestorProfile that holds
a row of data. InvestorProfileCollection holds the rows.

### Task Summary (to be completed by AI)
- **Changes Made**: Implemented InvestorProfile and InvestorProfileCollection classes to represent and manage investor profile data.

- **Files Created**: 
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/investor_profile/InvestorProfile.kt`
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/investor_profile/InvestorProfileCollection.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/investor_profile/InvestorProfileTest.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/investor_profile/InvestorProfileCollectionTest.kt`

- **Implementation Details**:
  - **InvestorProfile class**: 
    - Implemented as a data class to represent a single investor profile with id, name, description, and distributionPercentage
    - Added validation in the init block to ensure positive ID, non-empty name/description, and distribution percentage within 0-100 range
    - Used named arguments in constructor calls for better code readability

  - **InvestorProfileCollection class**:
    - Implemented methods to add, retrieve, and filter investor profiles
    - Created getById and getByName methods that throw IllegalArgumentException when profile not found (following defensive programming principles)
    - Added getByDistributionRange method with proper validation for percentage ranges
    - Added a companion object with createDefaultCollection() method that creates the predefined investor profiles from the design document (5 profiles: Conservative 25%, Balanced 40%, Aggressive 20%, Income 10%, Trend 5%)

- **Testing**:
  - Created comprehensive tests for both classes following the project's testing standards
  - Used one assertThat per test function and minimal logic in assertions as required
  - Tested validation rules, collection operations, filtering capabilities, and edge cases
  - Verified that the default collection contains the expected investor profiles with correct properties
  - Fixed one test that initially failed due to incorrect range expectations

- **Verification**: Successfully built and tested the implementation with all 167 tests passing, including 45 new tests for the investor profile classes.

- **Future Considerations**: The InvestorProfileCollection will be used by the User Generator to create users with different investment strategies and risk tolerances.

## Task 7 - Activity Levels
Generate a class that holds the activity level data. Create the class ActivityLevel that holds
a row of data. ActivityLevelCollection holds the rows.

### Task Summary (to be completed by AI)
- **Changes Made**: Implemented ActivityLevel and ActivityLevelCollection classes to represent and manage activity level data.

- **Files Created**: 
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/activity_level/ActivityLevel.kt`
  - `/src/main/kotlin/com/wisecrowd/data_generator/data_collections/activity_level/ActivityLevelCollection.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/activity_level/ActivityLevelTest.kt`
  - `/src/test/kotlin/com/wisecrowd/data_generator/data_collections/activity_level/ActivityLevelCollectionTest.kt`

- **Implementation Details**:
  - **ActivityLevel class**: 
    - Implemented as a data class to represent a single activity level with id, name, description, and distributionPercentage
    - Added validation in the init block to ensure positive ID, non-empty name/description, and distribution percentage within 0-100 range
    - Used named arguments in constructor calls for better code readability

  - **ActivityLevelCollection class**:
    - Implemented methods to add, retrieve, and filter activity levels
    - Created getById and getByName methods that throw IllegalArgumentException when level not found (following defensive programming principles)
    - Added getByDistributionRange method with proper validation for percentage ranges
    - Added a companion object with createDefaultCollection() method that creates the predefined activity levels from the design document (5 levels: Inactive 22%, Low 39%, Moderate 28%, Active 9%, Hyperactive 2%)

- **Testing**:
  - Created comprehensive tests for both classes following the project's testing standards
  - Used one assertThat per test function and minimal logic in assertions as required
  - Tested validation rules, collection operations, filtering capabilities, and edge cases
  - Verified that the default collection contains the expected activity levels with correct properties

- **Verification**: Successfully built and tested the implementation with all 212 tests passing, including 45 new tests for the activity level classes.

- **Future Considerations**: The ActivityLevelCollection will be used by the User Generator to create users with different trading frequencies and activity patterns.