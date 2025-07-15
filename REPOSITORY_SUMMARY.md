# HAR-to-K6 Repository Summary

## Overview

**har-to-k6** is a comprehensive tool for converting HTTP Archive (HAR) files and LoadImpact HAR (LI-HAR) files into K6 performance testing scripts. This enables users to transform captured network traffic into executable load tests for the K6 performance testing framework.

![Version](https://img.shields.io/badge/version-0.13.1-blue)
![License](https://img.shields.io/badge/license-Apache--2.0-green)
![Node.js](https://img.shields.io/badge/node-%3E%3D11.0.0-brightgreen)

## 🎯 Purpose & Use Cases

- **Performance Testing**: Convert real browser traffic into K6 load test scripts
- **Load Test Creation**: Generate scripts from HAR files captured during manual testing
- **CI/CD Integration**: Automate performance test creation from captured network data
- **API Testing**: Transform recorded API calls into repeatable performance tests

## 🏗️ Architecture

### Directory Structure
```
har-to-k6/
├── src/                    # Core functionality
│   ├── convert.js         # Main conversion logic
│   ├── validate/          # Input validation
│   ├── parse/             # HAR parsing
│   ├── render/            # K6 script generation
│   └── build/             # Browser bundle building
├── bin/                   # CLI implementation
├── test/                  # Test suites (unit, integration, e2e)
├── example/               # Sample HAR files
├── typings/               # TypeScript definitions
└── assets/                # Documentation assets
```

### Core Components

#### 1. **Converter (`src/convert.js`)**
- Main entry point for HAR to K6 conversion
- Orchestrates validation, parsing, and rendering
- Supports both single and multiple HAR file conversion
- Handles error reporting and validation failures

#### 2. **Validation (`src/validate/`)**
- Validates HAR archive structure and content
- Checks for required fields and data integrity
- Validates variables, sleep configurations, and options
- Ensures K6 compatibility

#### 3. **Parser (`src/parse/`)**
- Extracts and normalizes data from HAR files
- Processes HTTP requests, headers, and payloads
- Handles LI-HAR specific extensions
- Prepares data structure for script generation

#### 4. **Renderer (`src/render/`)**
- Generates K6 JavaScript code from parsed data
- Creates importable modules and exports
- Handles code formatting and prettification
- Supports various K6 features and configurations

## 🚀 Installation & Usage

### Installation Options

#### Local Installation (Recommended)
```bash
npm install --save har-to-k6
```

#### Global Installation
```bash
npm install --global har-to-k6
```

#### Docker
```bash
docker pull grafana/har-to-k6:latest
```

### Usage Examples

#### Command Line Interface
```bash
# Basic conversion
npx har-to-k6 archive.har -o my-k6-script.js

# From node_modules
./node_modules/.bin/har-to-k6 archive.har -o my-k6-script.js

# Global installation
har-to-k6 archive.har -o my-k6-script.js
```

#### Programmatic Usage
```javascript
const fs = require("fs");
const { liHARToK6Script } = require("har-to-k6");

async function convertHAR() {
  const archive = JSON.parse(fs.readFileSync("archive.har", "utf8"));
  const { main } = await liHARToK6Script(archive);
  fs.writeFileSync("./load-test.js", main);
}
```

#### Browser Usage
```javascript
// ES Module
import { liHARToK6Script } from "har-to-k6";

// CommonJS
const { liHARToK6Script } = require("har-to-k6");

// Script tag
// Load standalone.js and access via harToK6 global
```

#### Validation Only
```javascript
const { InvalidArchiveError, validate } = require("har-to-k6");

try {
  validate(archive);
  console.log("Archive is valid");
} catch (error) {
  if (error instanceof InvalidArchiveError) {
    console.error("Invalid archive:", error.message);
  }
}
```

## 📋 Features

### Core Features
- ✅ **HAR Support**: Standard HAR 1.2 format compatibility
- ✅ **LI-HAR Support**: LoadImpact extended HAR format
- ✅ **K6 Script Generation**: Complete K6 JavaScript output
- ✅ **Multi-format Support**: CLI, programmatic, and browser usage
- ✅ **Validation**: Comprehensive input validation
- ✅ **Error Handling**: Detailed error reporting

### Advanced Features
- ✅ **Variables**: Dynamic value substitution in requests
- ✅ **Checks**: HTTP response validation
- ✅ **Sleep/Think Time**: Configurable delays between requests
- ✅ **Headers Management**: Custom and computed headers
- ✅ **Post Data**: Form data and JSON payload support
- ✅ **Query Parameters**: URL parameter handling
- ✅ **Options**: K6 execution configuration

### K6 Integrations
- ✅ **HTTP Requests**: GET, POST, PUT, DELETE, etc.
- ✅ **Response Checks**: Status codes, content validation
- ✅ **Load Patterns**: Virtual users, duration, stages
- ✅ **Metrics**: Custom metrics and thresholds
- ✅ **Scenarios**: Advanced load testing scenarios

## 🛠️ Technical Stack

### Runtime & Language
- **Node.js**: >=11.0.0 (LTS recommended)
- **JavaScript**: ES6+ with Babel transpilation
- **Browser Support**: Chrome, Firefox, Safari, Edge (last 2 versions)

### Dependencies
- **CLI Framework**: Caporal for command-line interface
- **Validation**: Custom validation with JSONPath support
- **Formatting**: Prettier for code generation
- **Utilities**: Moment.js, nanoid, form-urlencoded
- **File System**: fs-extra for enhanced file operations

### Development Tools
- **Testing**: AVA test runner with comprehensive test suite
- **Linting**: ESLint with Prettier configuration
- **Building**: Webpack for browser bundle generation
- **CI/CD**: GitHub Actions for automated testing and releases

### Build System
```json
{
  "scripts": {
    "bundle": "webpack --config webpack.config.js",
    "lint": "eslint .",
    "test": "npm-run-all test-unit test-int test-e2e",
    "test-unit": "ava test/unit",
    "test-int": "ava test/int", 
    "test-e2e": "ava test/e2e"
  }
}
```

## 📚 Specifications

The project includes detailed specifications for each component:

### 1. **LI-HAR Format** (`li-har.spec.md`)
- JSON configuration format extending HAR 1.2
- K6-specific extensions and options
- Variables, checks, and advanced features
- Examples and usage patterns

### 2. **Converter Specification** (`converter.spec.md`)
- JavaScript package interface
- Browser and Node.js compatibility requirements
- API documentation and examples
- Input/output format specifications

### 3. **CLI Tool Specification** (`cli-tool.spec.md`)
- Command-line interface requirements
- Installation and usage instructions
- Cross-platform compatibility
- Integration with converter package

## 🧪 Testing

### Test Structure
```
test/
├── unit/          # Unit tests for individual components
├── int/           # Integration tests for component interaction
├── e2e/           # End-to-end tests with real HAR files
└── helper/        # Test utilities and fixtures
```

### Test Categories
- **Unit Tests**: Individual function and module testing
- **Integration Tests**: Component interaction validation
- **End-to-End Tests**: Complete workflow testing with sample HAR files
- **Fixture Tests**: Real-world HAR file conversion validation

### Running Tests
```bash
npm test                    # Run all tests
npm run test-unit          # Unit tests only
npm run test-int           # Integration tests only
npm run test-e2e           # End-to-end tests only
```

## 🔧 Development Setup

### Prerequisites
- Node.js >=11.0.0
- npm or yarn package manager
- Git for version control

### Setup Steps
```bash
# Clone repository
git clone https://github.com/grafana/har-to-k6.git
cd har-to-k6

# Install dependencies
npm install

# Run tests
npm test

# Build browser bundle
npm run bundle

# Lint code
npm run lint
```

### Known Issues
- **Node.js Compatibility**: Webpack build may fail on Node.js v18+ due to OpenSSL changes
- **Legacy Dependencies**: Some dependencies have deprecation warnings
- **Browser Bundle**: May require Node.js version downgrade for successful building

## 📖 Documentation

### Available Documentation
- **README.md**: Main project documentation and usage guide
- **ARCHITECTURE.md**: Technical architecture overview
- **CHANGELOG.md**: Version history and release notes
- **Specifications**: Detailed format and API specifications
- **Examples**: Sample HAR files and conversion examples

### API Documentation
The main API exports:
- `liHARToK6Script(archive, options)`: Main conversion function
- `validate(archive)`: Archive validation function
- `normalizeHAR(archive)`: HAR normalization utility
- `InvalidArchiveError`: Custom error class for validation failures

## 🎯 Example Use Cases

### 1. **API Load Testing**
```javascript
// Convert API HAR to K6 script
const { liHARToK6Script } = require("har-to-k6");
const apiHAR = require("./api-calls.har");

const { main } = await liHARToK6Script(apiHAR, {
  loadTest: true,
  includeStatic: false
});
```

### 2. **Web Application Testing**
```bash
# Full web application workflow
har-to-k6 webapp-session.har -o webapp-loadtest.js
```

### 3. **CI/CD Integration**
```javascript
// Automated test generation in build pipeline
const fs = require("fs");
const { liHARToK6Script, validate } = require("har-to-k6");

async function generateLoadTest(harFile) {
  const archive = JSON.parse(fs.readFileSync(harFile));
  validate(archive);
  const { main } = await liHARToK6Script(archive);
  return main;
}
```

## 🤝 Contributing

### Development Workflow
1. Fork the repository
2. Create feature branch
3. Make changes with tests
4. Run linting and tests
5. Submit pull request

### Code Standards
- ESLint configuration for code style
- Prettier for code formatting
- Comprehensive test coverage required
- Documentation updates for new features

## 📄 License & Credits

- **License**: Apache 2.0
- **Original Creator**: [bookmoons](https://github.com/bookmoons)
- **Current Maintainer**: Grafana/K6 team
- **Repository**: https://github.com/grafana/har-to-k6

## 🔗 Related Projects

- **K6**: https://docs.k6.io/ - Performance testing framework
- **HAR Specification**: https://w3c.github.io/web-performance/specs/HAR/Overview.html
- **Grafana**: https://grafana.com/ - Observability platform

---

*This summary provides a comprehensive overview of the har-to-k6 repository structure, functionality, and usage. For detailed technical information, refer to the individual specification files and source code documentation.*