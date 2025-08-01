# FLEXT dbt LDIF - Enterprise LDIF Data Analytics

[![Python 3.13+](https://img.shields.io/badge/python-3.13+-blue.svg)](https://www.python.org/downloads/)
[![dbt Core](https://img.shields.io/badge/dbt-1.6+-orange.svg)](https://getdbt.com)
[![Clean Architecture](https://img.shields.io/badge/Architecture-Clean%20Architecture%20%2B%20DDD-green.svg)](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
[![Coverage](https://img.shields.io/badge/coverage-90%25+-brightgreen.svg)](https://pytest.org)

Enterprise-grade dbt project for LDIF (LDAP Data Interchange Format) data transformations and analytics. Built with Python 3.13+, dbt Core, and programmatic model generation capabilities as part of the FLEXT enterprise data integration platform.

## Overview

FLEXT dbt LDIF provides advanced analytics, programmatic model generation, and comprehensive data transformations for LDAP directory data in LDIF format. The project features enterprise-grade quality controls with anomaly detection and risk assessment capabilities.

### Key Features

- **LDIF Analytics**: Comprehensive transformations for LDAP Data Interchange Format
- **Programmatic Model Generation**: Python-driven dbt model creation and management
- **Advanced Analytics**: Anomaly detection, pattern analysis, and risk assessment
- **Quality Controls**: 90% test coverage with zero-tolerance quality gates
- **PostgreSQL Backend**: Enterprise database with layered schema architecture
- **Clean Architecture**: Python components with domain-driven design patterns

## Quick Start

### Prerequisites

- **Python 3.13+** installed
- **dbt Core** with PostgreSQL adapter
- **LDIF data source** via flext-tap-ldif
- **Poetry** for dependency management

### Installation

```bash
# Clone and setup
git clone <repository-url>
cd flext-dbt-ldif

# Install dependencies
poetry install

# Setup database profiles
cp profiles/profiles.yml.example profiles/profiles.yml
# Edit profiles/profiles.yml with your PostgreSQL connection details

# Test connection
dbt debug
```

### First Run

```bash
# Install dbt dependencies
dbt deps

# Generate models programmatically
make generate-models

# Run all models
dbt run

# Run tests
dbt test

# Generate documentation
dbt docs generate
dbt docs serve
```

## Architecture

### Project Structure

```
   src/flext_dbt_ldif/           # Python package for programmatic features
      core.py                   # DBTModelGenerator and LDIFAnalytics classes
      cli.py                    # Command-line interface
      infrastructure/           # DI container and infrastructure code
   models/                       # dbt models (programmatically generated)
      staging/                  # Staging layer with schema validation
   profiles/                     # dbt profiles for PostgreSQL connection
   tests/                        # Python unit and integration tests
   dbt_project.yml              # dbt project configuration
   packages.yml                 # dbt package dependencies (dbt_utils, codegen)
   pyproject.toml               # Python dependencies and tool configuration
```

### Key Components

#### **DBTModelGenerator**

Programmatically generates dbt models for LDIF analytics with:

- Dynamic model creation based on LDIF schema
- Quality validation and testing integration
- Schema evolution and versioning support

#### **LDIFAnalytics**

Advanced analytics capabilities including:

- Pattern analysis and anomaly detection
- Data quality metrics and compliance checking
- Risk assessment and security audit features

### Data Modeling Layers

#### **Staging Layer** (`staging/`)

- **Purpose**: Raw LDIF data transformation and quality checks
- **Materialization**: Views for real-time processing
- **Key Model**: `stg_ldif_entries` with DN validation and categorization
- **Features**: Entry type classification, depth calculation, compliance validation

#### **Intermediate Layer** (`intermediate/`)

- **Purpose**: Business logic transformations and enrichment
- **Materialization**: Ephemeral models for optimal performance
- **Transformations**: Relationship mapping, hierarchy processing

#### **Marts Layer** (`marts/`)

- **Purpose**: Business-ready analytics models with advanced features
- **Materialization**: Tables for performance optimization
- **Key Model**: `analytics_ldif_insights` with time series and anomaly detection

## Development Commands

### Quality Gates (Zero Tolerance)

```bash
# Complete validation pipeline (run before commits)
make validate                # lint + type + security + test + dbt-test

# Essential checks
make check                   # lint + type + test + dbt-compile

# Individual quality gates
make lint                    # Ruff linting (ALL rules enabled)
make type-check              # MyPy strict mode
make security                # Bandit + pip-audit + detect-secrets
make format                  # Format code with Ruff
```

### dbt Operations

```bash
# Core dbt workflow
make dbt-deps                # Install dbt dependencies (dbt_utils, codegen)
make dbt-debug               # Debug dbt configuration and connections
make dbt-compile             # Compile dbt models without execution
make dbt-run                 # Execute dbt models
make dbt-test                # Run dbt data quality tests
make dbt-docs                # Generate dbt documentation

# LDIF-specific operations
make ldif-models-test        # Test LDIF-specific staging and marts models
make ldif-transformations    # Run LDIF transformation pipeline
make ldif-validate           # Validate LDIF data integrity
```

### Programmatic Model Generation

```bash
# Model generation workflow
make generate-models         # Generate dbt models programmatically
make update-schemas          # Update model schemas
make validate-generated      # Validate generated models

# Advanced generation
poetry run python -m flext_dbt_ldif generate --type staging
poetry run python -m flext_dbt_ldif generate --type marts --with-analytics
```

### Testing

```bash
# Complete test suite
make test                    # All tests with 90% coverage requirement
make test-unit               # Unit tests only
make test-integration        # Integration tests only
make coverage-html           # Generate HTML coverage report

# Test categories
pytest -m unit               # Unit tests
pytest -m integration        # Integration tests
pytest -m slow               # Slow tests
pytest -m "not slow"         # Exclude slow tests for fast feedback
```

## Configuration

### Database Connection

Configure PostgreSQL connection in `profiles/profiles.yml`:

```yaml
flext_ldif:
  target: dev
  outputs:
    dev:
      type: postgres
      host: localhost
      port: 5432
      user: postgres
      pass: password
      dbname: ldif_analytics
      schema: ldif_dev
      threads: 4
    prod:
      type: postgres
      host: "{{ env_var('DBT_POSTGRES_HOST') }}"
      port: "{{ env_var('DBT_POSTGRES_PORT') | int }}"
      user: "{{ env_var('DBT_POSTGRES_USER') }}"
      pass: "{{ env_var('DBT_POSTGRES_PASSWORD') }}"
      dbname: "{{ env_var('DBT_POSTGRES_DATABASE') }}"
      schema: "{{ env_var('DBT_POSTGRES_SCHEMA') }}"
      threads: 4
```

### Environment Variables

```bash
# Database configuration
export DBT_POSTGRES_HOST=localhost
export DBT_POSTGRES_USER=postgres
export DBT_POSTGRES_PASSWORD=password
export DBT_POSTGRES_DATABASE=ldif_analytics
export DBT_POSTGRES_SCHEMA=ldif_dev

# dbt settings
export DBT_PROFILES_DIR=profiles/
export DBT_TARGET=dev
export DBT_THREADS=4

# Analytics settings
export ANALYTICS_ENABLE_ANOMALY_DETECTION=true
export ANALYTICS_ENABLE_RISK_ASSESSMENT=true
```

### dbt Variables

Configure in `dbt_project.yml`:

```yaml
vars:
  # LDIF Processing Configuration
  ldif_base_dn: "dc=company,dc=com"
  ldif_entry_validation: strict
  enable_anomaly_detection: true
  enable_risk_assessment: true

  # Data Quality Thresholds
  data_quality_thresholds:
    completeness: 0.95
    accuracy: 0.98
    consistency: 0.90

  # Performance Settings
  enable_incremental_models: true
  incremental_lookback_days: 7
```

## Data Models

### Core LDIF Processing

#### Staging Model (`stg_ldif_entries`)

```sql
-- LDIF entries with validation and categorization
select
    {{ dbt_utils.generate_surrogate_key(['dn']) }} as entry_key,
    dn,
    {{ validate_dn_format('dn') }} as is_valid_dn,
    {{ calculate_dn_depth('dn') }} as dn_depth,
    {{ categorize_entry_type('object_class') }} as entry_type,
    object_class,
    attributes,
    {{ extract_base_dn('dn') }} as base_dn,
    created_timestamp,
    modified_timestamp,
    file_source,
    line_number,
    current_timestamp as dbt_updated_at
from {{ source('ldif_raw', 'entries') }}
where dn is not null
```

#### Analytics Model (`analytics_ldif_insights`)

```sql
-- Advanced LDIF analytics with anomaly detection
select
    entry_key,
    dn,
    entry_type,
    dn_depth,
    base_dn,

    -- Time series analytics
    {{ generate_time_series_metrics('created_timestamp') }} as time_metrics,

    -- Anomaly detection
    {{ detect_anomalies('attributes', 'entry_type') }} as anomaly_score,

    -- Risk assessment
    {{ assess_security_risk('dn', 'object_class') }} as risk_level,

    -- Pattern analysis
    {{ analyze_patterns('attributes') }} as patterns,

    current_timestamp as analysis_timestamp
from {{ ref('stg_ldif_entries') }}
```

### Custom Macros

#### LDIF Validation Macros

```sql
-- Validate DN format compliance
{{ validate_dn_format('dn_column') }}

-- Calculate DN hierarchy depth
{{ calculate_dn_depth('dn_column') }}

-- Categorize entry types
{{ categorize_entry_type('object_class_column') }}

-- Extract base DN
{{ extract_base_dn('dn_column') }}
```

#### Analytics Macros

```sql
-- Generate time series metrics
{{ generate_time_series_metrics('timestamp_column') }}

-- Detect data anomalies
{{ detect_anomalies('data_column', 'category_column') }}

-- Assess security risk
{{ assess_security_risk('dn_column', 'object_class_column') }}

-- Analyze data patterns
{{ analyze_patterns('attributes_column') }}
```

## Testing Strategy

### Data Quality Tests

#### Schema Tests (Built-in dbt tests)

```yaml
# models/staging/schema.yml
models:
  - name: stg_ldif_entries
    tests:
      - unique:
          column_name: dn
      - not_null:
          column_name: [dn, entry_type, object_class]
    columns:
      - name: dn
        tests:
          - not_null
          - unique
      - name: is_valid_dn
        tests:
          - accepted_values:
              values: [true, false]
```

#### LDIF-Specific Tests

```sql
-- tests/staging/test_stg_ldif_dn_format.sql
-- Test: All DNs should follow RFC 4514 format
select *
from {{ ref('stg_ldif_entries') }}
where dn is not null
  and not {{ validate_dn_format('dn') }}
```

#### Analytics Tests

```sql
-- tests/marts/test_analytics_ldif_quality.sql
-- Test: Analytics results should meet quality thresholds
select *
from {{ ref('analytics_ldif_insights') }}
where anomaly_score > 0.9  -- High anomaly threshold
   or risk_level = 'CRITICAL'
```

### Python Tests

```bash
# Run Python component tests
pytest tests/

# Test specific components
pytest tests/test_core.py -v           # Test DBTModelGenerator and LDIFAnalytics
pytest tests/test_cli.py -v            # Test CLI functionality
pytest tests/test_infrastructure.py -v # Test DI container
```

### Running Tests

```bash
# All tests
make test                        # Python + dbt tests with coverage

# dbt tests only
dbt test                         # All dbt tests
dbt test --select staging       # Staging models only
dbt test --data                  # Data tests only

# Python tests only
pytest                           # All Python tests
pytest -m unit                   # Unit tests only
pytest -m integration           # Integration tests only

# Test coverage
make coverage-html              # HTML report in reports/coverage/
```

## Advanced Features

### Programmatic Model Generation

The `DBTModelGenerator` class provides powerful model generation capabilities:

```python
from flext_dbt_ldif.core import DBTModelGenerator

# Initialize generator
generator = DBTModelGenerator()

# Generate staging models
generator.generate_staging_models(
    source_schema="ldif_raw",
    target_schema="ldif_staging"
)

# Generate analytics models with advanced features
generator.generate_analytics_models(
    enable_anomaly_detection=True,
    enable_risk_assessment=True,
    time_series_analysis=True
)
```

### Anomaly Detection

The platform includes advanced anomaly detection capabilities:

- **Statistical Anomalies**: Detect unusual patterns in LDIF data
- **Structural Anomalies**: Identify malformed DN structures
- **Temporal Anomalies**: Detect unusual timing patterns
- **Security Anomalies**: Identify potential security risks

### Risk Assessment

Comprehensive risk assessment features:

- **DN Risk Analysis**: Assess risks based on DN patterns
- **Attribute Risk Scoring**: Score risks based on attribute combinations
- **Access Pattern Analysis**: Identify unusual access patterns
- **Compliance Checking**: Validate against security policies

## Performance Optimization

### LDIF Performance

- **Incremental Processing**: For large LDIF files with millions of entries
- **Partitioning**: By base DN or entry type for optimal query performance
- **Indexing**: Proper indexing on DNs, entry types, and frequently queried attributes
- **Streaming Processing**: Efficient processing of large LDIF files

### dbt Performance

- **Materialization Strategy**: Views for staging, tables for marts
- **Macro Optimization**: Efficient LDIF-specific transformations
- **Parallel Processing**: Optimized dbt thread configuration
- **Resource Management**: Memory-efficient processing of large datasets

### Best Practices

```yaml
# dbt_project.yml - Performance configuration
models:
  flext_dbt_ldif:
    staging:
      +materialized: view
    intermediate:
      +materialized: ephemeral
    marts:
      +materialized: table
      +indexes:
        - columns: ["dn"]
        - columns: ["entry_type"]
        - columns: ["base_dn"]
```

## Deployment

### Environment Setup

#### Development

```bash
# Local development with PostgreSQL
dbt run --target dev
dbt test --target dev
```

#### Production

```bash
# Production deployment
dbt run --target prod
dbt test --target prod --store-failures
dbt docs generate --target prod
```

### CI/CD Pipeline

```yaml
# .github/workflows/dbt-ldif.yml
name: dbt LDIF Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.13"
      - name: Install dependencies
        run: poetry install
      - name: Run quality checks
        run: make validate
      - name: Run dbt tests
        run: dbt test --target ci
```

## Quality Standards

### Zero Tolerance Quality Gates

- **Test Coverage**: Minimum 90% Python test coverage
- **Type Safety**: Strict MyPy with no untyped code
- **Linting**: Ruff with ALL rules enabled
- **Security**: Bandit scanning + pip-audit for vulnerabilities
- **dbt Tests**: All data quality tests must pass
- **Pre-commit**: Automated quality checks on every commit

### Code Standards

- **Python 3.13+**: Latest Python with modern type hints
- **Clean Architecture**: Strict layer separation following FLEXT patterns
- **Domain-Driven Design**: Rich domain entities with business logic
- **dbt Best Practices**: Proper layering, testing, and documentation

## Dependencies

### FLEXT Ecosystem Dependencies

- **flext-core**: Base patterns and utilities
- **flext-ldif**: LDIF processing capabilities
- **flext-meltano**: dbt/Singer/Meltano integration
- **flext-observability**: Monitoring and logging

### External Dependencies

- **dbt-core**: Data transformation framework
- **dbt-postgres**: PostgreSQL adapter
- **dbt-utils**: Utility macros for data transformations
- **pydantic**: Data validation and settings management
- **click**: CLI framework
- **rich**: Enhanced terminal output

## Troubleshooting

### Common dbt Issues

```bash
# Connection problems
make dbt-debug                   # Check profiles and connections

# Compilation errors
make dbt-compile                 # Test model compilation
dbt compile --select model_name

# Test failures
make dbt-test                    # Run all dbt tests
dbt test --select model_name

# Performance issues
dbt run --select model_name --full-refresh
```

### Python Integration Issues

```bash
# Import errors
poetry run python -c "import flext_dbt_ldif"

# Dependency conflicts
poetry show --tree
poetry update

# Generation issues
make validate-generated
```

### Quality Gate Failures

```bash
# Fix linting automatically
make format

# Type checking issues
poetry run mypy src/ --show-error-codes

# Security vulnerabilities
poetry run pip-audit --fix

# Test coverage below 90%
make coverage-html              # View detailed coverage report
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-ldif-analytics`)
3. Develop and test changes
4. Run quality gates (`make validate`)
5. Update documentation
6. Commit changes (`git commit -m 'Add new LDIF analytics model'`)
7. Push to branch (`git push origin feature/new-ldif-analytics`)
8. Open a Pull Request

## License

MIT License - see [LICENSE](LICENSE) for details.

## Documentation

### Architecture & Development

- [CLAUDE.md](CLAUDE.md) - Development guidance and architectural patterns
- [docs/](docs/) - Comprehensive project documentation

### Generated Documentation

```bash
# Generate and serve dbt docs
dbt docs generate
dbt docs serve --port 8080

# View at: http://localhost:8080
```

### Related Projects

- [../../flext-core/](../../flext-core/) - Foundation library with shared patterns
- [../../flext-ldif/](../../flext-ldif/) - LDIF processing infrastructure
- [../../flext-tap-ldif/](../../flext-tap-ldif/) - Singer tap for LDIF data extraction
- [../../flext-target-ldif/](../../flext-target-ldif/) - Singer target for LDIF export

### Ecosystem Integration

- [../../flext-meltano/](../../flext-meltano/) - Orchestration platform
- [../../flext-observability/](../../flext-observability/) - Monitoring and logging

---

**Framework**: FLEXT Ecosystem | **Technology**: dbt Core + Python 3.13+ | **Architecture**: Clean Architecture + DDD | **Updated**: 2025-07-30
