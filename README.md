# Reusable GitHub Actions

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](./LICENSE)

This repository provides a collection of **reusable GitHub Actions** designed to help developers simplify and standardize their CI/CD workflows.

## Project Goals

By providing validated, reusable GitHub Actions, we enable teams to:

- 🚀 **Rapid Integration** - Use pre-built Actions directly without rewriting from scratch
- 🔄 **Standardized Processes** - Ensure all projects use consistent build and deployment workflows
- 📦 **Reduced Maintenance** - Centrally manage common workflows and eliminate duplicated code
- ✨ **Increased Productivity** - Focus on business logic instead of infrastructure configuration

## Available Actions

This repository contains reusable Actions of the following types:

| Category | Description | Status |
|----------|-------------|--------|
| Build | Build workflows for various programming languages | 🚧 In Development |
| Test | Automated test execution | 🚧 In Development |
| Deploy | Multi-environment deployment processes | 🚧 In Development |
| Code Quality | Linting, formatting, and security scanning | 🚧 In Development |

## Quick Start

### Using in Your Project

Create workflow files in your `.github/workflows/` directory and reference these Actions using the `uses` statement:

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      # Use reusable build Action
      - uses: ./path/to/reusable-action/build
        with:
          node-version: '18'

      # Use reusable test Action
      - uses: ./path/to/reusable-action/test
        with:
          coverage: true
```

## Directory Structure

```
.
├── .github/
│   └── workflows/          # GitHub Actions workflows
├── actions/                # Reusable Actions
│   ├── build/              # Build-related Actions
│   ├── test/               # Test-related Actions
│   ├── deploy/             # Deployment Actions
│   └── shared/             # Shared helper scripts and utilities
├── docs/                   # Documentation
└── README.md
```

## Development Guide

### Creating a New Reusable Action

1. Create a new Action directory under `actions/`
2. Add an `action.yml` or `action.yaml` configuration file
3. Implement the Action logic (JavaScript/TypeScript/Composite)
4. Write documentation and examples
5. Add corresponding tests

### Testing Actions

Before submitting, ensure your Actions are tested:

```bash
# Run local tests
npm test

# Validate syntax
actionlint
```

## Best Practices

- ✅ **Single Responsibility** - Each Action should handle one specific task
- ✅ **Parameterized** - Use `inputs` to make Actions flexible
- ✅ **Well Documented** - Clearly explain usage and parameters
- ✅ **Error Handling** - Provide clear error messages and exit codes
- ✅ **Version Pinning** - Use fixed version tags in production

## Contributing

Issues and Pull Requests are welcome!

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed information.

## License

This project is licensed under the Apache 2.0 License - see the [LICENSE](./LICENSE) file for details.

## Support

If you have any questions or suggestions:

- Submit an [Issue](../../issues)
- Check the [Documentation](./docs/)
- Contact the maintainers

---

**Making CI/CD simpler and development more efficient!** 🎉
