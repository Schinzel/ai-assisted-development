# Technology Stack

## Overview
This document describes the technology stack used in the application. It serves as a reference for both human developers and AI assistants working on the codebase.

## Backend

### Programming Language
- **Kotlin** - Primary programming language
  - Version: 1.8.x
  - JVM-based with modern language features
  - Null safety, extension functions, and coroutines

### Architecture
- **Modular monolith** - Organized into cohesive modules with clear boundaries
- **Domain-driven design** principles
- **Port and adapter architecture** (Hexagonal) for core domains

### Database
- **MySQL** - Primary relational database
  - Used for structured data storage

### Search
- **OpenSearch** - For text search functionality
  - Used for recipe search, ingredient search, and similar features
  - Handles decomposition of compound words (especially important for Nordic languages)
  - Manages synonyms for improved search results

### Web Framework
- **Javalin** - Lightweight web framework for Kotlin/Java
  - Used for REST API endpoints and serving web pages
  - Embedded for simplicity

### File Storage
For images and other media files
  - Interfaces with a content delivery network for performance

## Frontend

### Web Technologies
- **HTML5/CSS3/JavaScript**
- **Bootstrap** - UI framework for responsive design
- **jQuery** - Used for DOM manipulation and AJAX calls

### Rich Text Editing
- **Summernote** - For content editing (recipes, descriptions)

## Development & Operations

### Build Tool
- **Maven** - For dependency management and build processes
  - Multi-module project structure

### Version Control
- **Git**

### Testing
- **JUnit 5** - For unit and integration tests
- **Mockito** - For mock objects in tests

### CI/CD
- **GitHub Actions** - For continuous integration and delivery

## Libraries & Utilities

### Logging
- Custom logging framework built on top of standard JVM logging

### Validation
- Custom validation framework for input validation

### Measurement Conversions
- Custom unit conversion system for recipe ingredients

## Infrastructure
- **Docker** - For containerization and local development environment
- **Docker Compose** - For multi-container Docker applications