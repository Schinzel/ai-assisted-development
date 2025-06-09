### Architecture
- **Feature based architecture**
- **Modular monolith** - Organized into cohesive modules with clear boundaries
- **Domain-driven design** principles


The data generators have data savers. The  


## Component Integration Pattern
The system uses a DataGenerationService to orchestrate the interaction between IDataGenerator and IDataSaver:
- IDataGenerator: Handles data generation logic
- IDataSaver: Handles file output operations
- DataGenerationService: Coordinates the generate-and-save workflow
  This ensures single responsibility and testability.

## Package Structure
All data generators must be placed in the `data_generators` package:
- `com.wisecrowd.data_generator.data_generators` for generator implementations
- Follow the existing pattern established with `data_collections` package