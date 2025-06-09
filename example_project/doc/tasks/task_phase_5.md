# Phase 5 - Main Orchestrator

# Current Implementation Status (to be completed by AI)

# Tasks

## Phase 5 - Task 1 - Centralize File Format Specifications
Create centralized constants for file format specifications to eliminate duplication between readers and writers.

### Description
Extract file format constants from FileDataSaver into a centralized location that can be shared by all file reading and writing components. This eliminates duplication and ensures consistency across the entire file I/O system.

### Current State
- FileDataSaver has ROW_DELIMITER, COLUMN_DELIMITER, STRING_QUALIFIER as private constants
- File format specifications are duplicated in task documentation
- Future FileDataParser will need the same constants

### Target State
- Single source of truth for all file format specifications
- Shared constants accessible by both readers and writers
- No duplication of format specifications

### Deliverables
- FileFormatConstants object with centralized constants
- Updated FileDataSaver to use centralized constants
- Updated any existing tests that reference format constants

### Acceptance Criteria
1. Create FileFormatConstants object in appropriate package
2. Define ROW_DELIMITER = "\n", COLUMN_DELIMITER = "\t", STRING_QUALIFIER = "###"
3. Define HAS_HEADER_ROW = true (first row contains column names)
4. Update FileDataSaver to use FileFormatConstants instead of private constants
4. Ensure backward compatibility - no behavior changes
5. Update unit tests to use centralized constants where needed
6. Place in package accessible by both data_saver and future file parsing utilities
7. All existing tests continue to pass

### Task Summary (to be completed by AI)

## Phase 5 - Task 2 - Refactor Error Handling to DataGenerationService
Move error collection and handling from FileDataSaver to DataGenerationService for centralized error management.

### Description
Refactor the error handling system to centralize error collection in DataGenerationService. This will catch errors from both IDataGenerator and IDataSaver components and work with any IDataSaver implementation, not just FileDataSaver.

### Current State
- FileDataSaver has error collection via getErrors() and hasErrors() methods
- DataGenerationService doesn't check for or handle errors
- IDataGenerator errors can bubble up uncaught
- Error handling is tied to FileDataSaver implementation

### Target State
- DataGenerationService collects and manages all errors from both generator and saver
- IDataSaver interface simplified (no error handling methods)
- Works with any IDataSaver implementation
- Centralized error reporting and decision making

### Deliverables
- Updated DataGenerationService with error collection
- Updated IDataSaver interface (remove error methods)
- Updated FileDataSaver (remove error handling)
- Updated unit tests for all affected classes

### Acceptance Criteria
1. DataGenerationService has getErrors() and hasErrors() methods
2. DataGenerationService catches and collects errors from both IDataGenerator and IDataSaver
3. IDataSaver interface no longer has error handling methods
4. FileDataSaver simplified - no more error collection
5. Existing error handling behavior preserved (small errors collected, fatal errors thrown)
6. All unit tests updated and passing
7. Integration tests verify error collection works with different IDataSaver implementations

### Task Summary (to be completed by AI)

## Phase 5 - Task 3 - File Parsing Utility
Create utility to parse generated files back into List<List<String>> format for generator dependencies.

### Description
Implement a utility class that can read the generated tab-delimited files and convert them into the List<List<String>> format expected by dependent generators (TransactionDataGenerator, UserHoldingsDataGenerator).

### Deliverables
- FileDataParser utility class
- Support for parsing all generated file types
- Integration with FileFormatConstants

### Acceptance Criteria
1. FileDataParser can read files using FileFormatConstants specifications
2. Correctly skips header row based on FileFormatConstants.HAS_HEADER_ROW
3. Returns List<List<String>> format compatible with existing generators
4. Handles empty files and malformed data gracefully
5. Uses FileFormatConstants for consistency (ROW_DELIMITER, COLUMN_DELIMITER, STRING_QUALIFIER)
6. Comprehensive unit tests covering normal operation and edge cases
7. Performance optimized for large files (thousands of assets/users)

### Task Summary (to be completed by AI)

## Phase 5 - Task 4 - Data Generation Configuration
Create DataGenerationConfig class to manage all configuration parameters for the data generation process.

### Description
Implement a configuration class that holds all parameters needed for data generation with sensible defaults and automatic output directory creation.

### Configuration Parameters
- startDate: LocalDate (default: 2020-01-01)
- endDate: LocalDate (default: current date)  
- numberOfAssets: Int (default: 100)
- numberOfUsers: Int (default: 1,000)
- outputDirectory: String (auto-generated with timestamp)

### Output Directory Format
- Location: User's Desktop
- Format: wise_crowd_data_{month}_{day}_{hour}:{minute}
- Example: wise_crowd_data_jun_07_15:59
- Timezone: CET
- Month: lowercase (jan, feb, mar, etc.)
- Time: 24-hour format

### Deliverables
- DataGenerationConfig data class
- Automatic timestamp generation in CET timezone
- Default value validation

### Acceptance Criteria
1. DataGenerationConfig data class with all specified parameters and defaults
2. Automatic output directory creation with correct timestamp format
3. Uses CET timezone for timestamp generation
4. Lowercase month names (jan, feb, mar, etc.)
5. 24-hour time format (15:59, not 3:59pm)
6. Input validation for date ranges (endDate >= startDate)
7. Input validation for positive numbers (numberOfAssets > 0, numberOfUsers > 0)
8. Unit tests covering default creation, custom parameters, and validation
9. Desktop path detection works across different operating systems

### Task Summary (to be completed by AI)

## Phase 5 - Task 5 - Main Orchestrator Implementation
Create WiseCrowdDataGenerator class that orchestrates the complete data generation pipeline.

### Description
Implement the main orchestrator that coordinates all data generators in the correct sequence, handles file dependencies, and provides detailed progress feedback with timing information.

### Generation Sequence
1. Generate asset_data.txt (AssetDataGenerator)
2. Generate price_series.txt (PriceSeriesDataGenerator - reads asset_data.txt)
3. Generate users.txt (UserDataGenerator)  
4. Generate transactions.txt (TransactionDataGenerator - reads price_series.txt + users.txt)
5. Generate user_holdings.txt (UserHoldingsDataGenerator - reads transactions.txt)

### Progress Feedback Format
```
Step 1/5: Generating 100 assets...
Step 1 completed in 245ms
Step 2/5: Generating price series for 100 assets over 1,826 days...
Step 2 completed in 1,423ms (3 warnings)
...
All steps completed in 3,891ms

Warnings encountered:
- Step 2: Invalid price data for asset ABC123, using fallback value
- Step 4: Transaction amount exceeds threshold for user XYZ789
- Step 4: Currency conversion failed for transaction 456, skipped
```

### Error Handling Strategy
- Fail fast on any fatal error
- Clean up all partial files on failure
- Collect and report small errors from DataGenerationService after each step
- Display warning counts in step completion messages
- Show detailed error summary at the end if any warnings occurred

### Deliverables
- WiseCrowdDataGenerator main orchestrator class
- ILog interface for output handling
- SystemOutLog default implementation
- Integration with all existing generators
- File dependency handling using FileDataParser
- Progress reporting with timing

### Acceptance Criteria
1. Create ILog interface with writeToLog(message: String) method
2. Create SystemOutLog class implementing ILog that writes to System.out
3. WiseCrowdDataGenerator takes DataGenerationConfig and ILog as constructor parameters
4. ILog defaults to SystemOutLog() for backward compatibility
5. Single generate() method orchestrates complete pipeline
6. Generates all 5 files in correct dependency order
4. Uses FileDataParser to read intermediate files for dependent generators
5. Creates output directory if it doesn't exist
6. Detailed progress feedback with step numbers, descriptions, timing, and warning counts
7. All progress messages use ILog interface instead of direct println()
8. Fail fast error handling with partial file cleanup
8. Collect and report errors from DataGenerationService after each step
9. Display warning summary at end if any errors were collected
10. Integration with DataGenerationService for each generator
11. Comprehensive unit tests with mocked components and TestLog for output verification
12. Integration test generating actual files with small dataset
13. Error reporting tests verify warning collection and display

### Task Summary (to be completed by AI)

## Phase 5 - Task 7 - Update README with Usage Examples
Update the main README.md file to include basic usage documentation for the completed data generation system.

### Description
Add a simple usage section to the README explaining the purpose, generated files, and how to run the data generator.

### Content to Add
- Brief purpose statement
- List of 5 generated files with descriptions
- Basic usage example showing how to generate data

### Deliverables
- Updated README.md with usage section

### Acceptance Criteria
1. Add "Usage" section to README.md
2. Explain purpose: generates mock data for WiseCrowd platform
3. List all 5 generated files (asset_data.txt, price_series.txt, users.txt, transactions.txt, user_holdings.txt) with brief descriptions
4. Show basic code example of how to generate data
5. Keep existing content (code standards links, project description links)
6. Simple and concise - no advanced scenarios or troubleshooting

### Task Summary (to be completed by AI)

## Phase 5 - Task 8 - Integration Testing & Final Verification
Create comprehensive integration tests and verify the complete data generation pipeline works end-to-end.

### Description
Implement integration tests that verify the complete data generation pipeline produces valid, consistent data files that meet the WiseCrowd platform requirements.

### Test Scenarios
- Small dataset generation (10 assets, 50 users)
- Custom date ranges and parameters
- Error scenarios and recovery

### Data Validation Requirements
- All files have correct format and headers
- Asset IDs are consistent across asset_data.txt and price_series.txt
- User IDs are consistent across users.txt, transactions.txt, and user_holdings.txt
- Price series covers complete date range for all assets
- All generated files are within the expected ranges of rows or data
- User holdings match transaction history
- No orphaned references between files

### Deliverables
- Comprehensive integration test suite
- Data validation utilities
- End-to-end pipeline verification

### Acceptance Criteria
1. Integration tests cover small dataset scenarios and custom configurations
2. Data consistency validation across all generated files
3. Error scenario testing (invalid config, disk space, etc.)
4. Documentation for running complete data generation process
5. Verification that all original design requirements are met

### Task Summary (to be completed by AI)
