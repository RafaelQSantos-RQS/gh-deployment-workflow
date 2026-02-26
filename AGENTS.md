# AGENTS.md - gh-deployment-workflow

## Project Overview

This is an educational repository designed to teach CI/CD concepts using GitHub Actions and GitHub Pages. The goal is to learn how to write a GitHub Actions workflow that deploys a static website to GitHub Pages.

The project demonstrates core CI/CD concepts:
- **Continuous Integration (CI)**: Automatically running workflows when code changes
- **Continuous Deployment (CD)**: Automatically deploying changes to production
- **GitHub Actions**: GitHub's native CI/CD platform
- **GitHub Pages**: Static site hosting service

## What This Project Is

- **Purpose**: Learning project for continuous integration and deployment
- **Content**: Contains a README explaining GitHub Actions workflow concepts
- **Expected Files**: An `index.html` file and `.github/workflows/deploy.yml` would be created by learners
- **Current State**: Minimal repository with only README.md and LICENSE

## Build/Lint/Test Commands

This is not a code repository - there are no build, lint, or test commands.

### For Future Extensions

If extending this project with actual code, typical commands would include:

```bash
# Local development (if using a static site generator)
npm run dev

# Build for production
npm run build

# Run linter
npm run lint

# Run tests
npm test

# Run a single test file
npm test -- path/to/test/file.test.js

# Run tests in watch mode
npm test -- --watch

# Run tests with coverage
npm test -- --coverage
```

### GitHub Actions Workflow Testing

To test workflows locally, consider using:
- **act**: Run GitHub Actions locally (https://github.com/nektos/act)
- **actionlint**: Lint GitHub Actions workflows (https://github.com/rhysd/actionlint)

```bash
# Install act (macOS)
brew install act

# Run workflow locally
act

# Lint workflow files
actionlint
```

## Code Style Guidelines

Since this is an educational project with no source code, there are no formal style guidelines. The following conventions apply if code is added.

### General Principles

- Keep code simple and readable for educational purposes
- Add comments explaining CI/CD concepts
- Use meaningful variable and function names
- Avoid over-abstraction in learning materials

### Language-Specific Guidelines

#### HTML
- Use semantic HTML5 elements
- Keep indentation at 2 spaces
- Use descriptive IDs and classes

#### YAML (Workflow Files)
- Use 2 spaces for indentation (no tabs)
- Use trailing commas in sequences
- Use quotes around strings with special characters
- Order keys alphabetically within sections

#### JavaScript/TypeScript (if extending)
- Use ES6+ features
- Prefer const over let, avoid var
- Use arrow functions for callbacks
- Use template literals instead of string concatenation
- Add TypeScript types when extending functionality

### Naming Conventions

- **Files**: kebab-case (e.g., `deploy.yml`, `my-workflow.yml`)
- **Functions**: camelCase (e.g., `buildSite()`, `deployPages()`)
- **Classes**: PascalCase (e.g., `WorkflowRunner`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `MAX_RETRIES`)
- **Workflow names**: Descriptive, Title Case (e.g., "Deploy to GitHub Pages")
- **Job names**: Descriptive, lowercase (e.g., `build`, `deploy`)
- **Step names**: Descriptive, Title Case (e.g., "Setup Node.js")

### Import Conventions

For JavaScript/TypeScript:
```javascript
// External libraries first
import React from 'react';
import axios from 'axios';

// Internal modules second
import { utils } from './utils';
import { config } from '../config';

// Relative paths for local imports
import './styles.css';
```

### Formatting

- Use 2 spaces for indentation in YAML, HTML
- Use Prettier for JavaScript/TypeScript formatting
- Maximum line length: 100 characters where practical
- Add newlines between logical sections
- Use trailing commas in arrays and objects

### Error Handling

For JavaScript code:
```javascript
// Use try-catch for async operations
async function deploy() {
  try {
    await uploadArtifact();
    await triggerDeployment();
  } catch (error) {
    console.error('Deployment failed:', error.message);
    process.exit(1);
  }
}

// Use proper error types
class DeploymentError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.name = 'DeploymentError';
    this.statusCode = statusCode;
  }
}
```

For GitHub Actions workflows:
- Use `if: ${{ failure() }}` for cleanup steps
- Add timeout-minutes to prevent hung jobs
- Use conditional steps for error recovery
- Enable verbose logging for debugging

## GitHub Actions Workflow Guidelines

### Basic Deployment Workflow

```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
    paths: ['index.html']

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### Workflow Best Practices

1. **Trigger on specific paths**: Use `paths` to only run when relevant files change
2. **Set appropriate permissions**: Use minimum required permissions
3. **Use latest action versions**: Update actions periodically for security
4. **Add timeout limits**: Prevent stuck jobs from wasting resources
5. **Use concurrency groups**: Cancel outdated runs for same branch
6. **Add workflow_dispatch**: Allow manual triggering for testing

### Advanced Workflow Example

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
    paths: ['index.html', 'styles/**', 'scripts/**']
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: 'pages'
  cancel-in-progress: true

jobs:
  deploy:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Validate HTML
        run: |
          if [ ! -f index.html ]; then
            echo "Error: index.html not found"
            exit 1
          fi

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### Common Workflow Patterns

#### Running Multiple Jobs
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
      - name: Deploy
        uses: actions/deploy-pages@v4
```

#### Matrix Builds
```yaml
jobs:
  test:
    strategy:
      matrix:
        node-version: [16, 18, 20]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm test
```

## Cursor/Copilot Rules

No Cursor or Copilot rules exist in this repository.

## Directory Structure

If learners complete the project, the expected structure would be:

```
gh-deployment-workflow/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── index.html
├── README.md
├── LICENSE
└── AGENTS.md (this file)
```

## Testing Guidelines

Since there is no test suite currently:

1. **Manual Testing**: Test workflows by pushing changes to a fork
2. **act Tool**: Use `act` to run workflows locally
3. **actionlint**: Validate workflow YAML syntax
4. **Branch Protection**: Test that workflows only trigger on main

### If Adding Tests

For JavaScript/TypeScript:
- Use Jest as the test framework
- Follow Arrange-Act-Assert pattern
- Name test files with `.test.js` or `.spec.js` suffix
- Put tests in `__tests__/` directory or alongside source files

```javascript
describe('deploy', () => {
  beforeEach(() => {
    // Setup
  });

  it('should deploy successfully', async () => {
    // Arrange
    const mockArtifact = { path: 'dist/' };

    // Act
    const result = await deploy(mockArtifact);

    // Assert
    expect(result.success).toBe(true);
  });
});
```

## Notes for Agents

- This repository is meant as a learning resource, not production code
- The README.md contains the full educational content
- Any changes should maintain the educational purpose
- Do not add complex build systems or testing frameworks unless explicitly requested
- Do not add actual code files (like index.html) unless needed for the workflow demonstration
- Focus on documentation and workflow examples that teach CI/CD concepts
- If asked to add code, prefer simple, educational examples over production-quality implementations
