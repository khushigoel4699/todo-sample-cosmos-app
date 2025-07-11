# Test Setup and GitHub Actions

This repository includes comprehensive end-to-end testing using Playwright and GitHub Actions workflows.

## 📁 Files Created

### GitHub Actions Workflows

1. **`.github/workflows/playwright-tests.yml`** - Full automated testing workflow
   - Runs on push to main/develop branches and pull requests
   - Sets up complete environment (API + Frontend)
   - Runs Playwright tests
   - Uploads test reports and results

2. **`.github/workflows/e2e-tests-manual.yml`** - Manual testing workflow
   - Triggered manually with workflow_dispatch
   - Tests against any provided URL
   - Useful for testing deployed environments

### Local Test Scripts

3. **`run-tests.sh`** - Bash script for Linux/macOS
4. **`run-tests.bat`** - Batch script for Windows

## 🚀 Running Tests Locally

### Prerequisites
- .NET SDK 8.0
- Node.js 18+
- curl (for health checks)

### Option 1: Using Test Scripts

**Linux/macOS:**
```bash
chmod +x run-tests.sh
./run-tests.sh
```

**Windows:**
```cmd
run-tests.bat
```

### Option 2: Manual Setup

1. **Start the API:**
```bash
cd src/api
dotnet run
```

2. **Start the Frontend (in another terminal):**
```bash
cd src/web
npm run dev
```

3. **Run Tests (in another terminal):**
```bash
cd tests
npm install
npx playwright install --with-deps
REACT_APP_WEB_BASE_URL=http://localhost:5173 npx playwright test
```

## 🔧 Test Configuration

### Environment Variables

- `REACT_APP_WEB_BASE_URL` - Base URL for the web application (default: http://localhost:3000)
- `CI` - Set to true in CI environments for optimal CI configuration

### Playwright Configuration

The tests are configured in `tests/playwright.config.ts`:
- **Timeout**: 2 hours per test (configurable for long-running operations)
- **Retries**: 2 retries on CI, 0 locally
- **Browsers**: Chromium (can be extended to Firefox, Safari)
- **Reporter**: HTML report with detailed test results

## 📊 Test Reports

After running tests, you can view detailed reports:

```bash
cd tests
npx playwright show-report
```

This will open an interactive HTML report showing:
- Test results and timings
- Screenshots and videos of failures
- Trace files for debugging
- Network activity and console logs

## 🔄 GitHub Actions Triggers

### Automatic Triggers (playwright-tests.yml)
- Push to `main` or `develop` branches
- Pull requests targeting `main` branch
- Manual trigger via workflow_dispatch

### Manual Trigger (e2e-tests-manual.yml)
- Go to Actions tab in GitHub
- Select "E2E Tests (Manual)"
- Click "Run workflow"
- Provide the URL to test against

## 🐛 Troubleshooting

### Common Issues

1. **Port conflicts**: Make sure ports 3100 (API) and 3000 (Frontend) are free
2. **Database connection**: The API requires Cosmos DB configuration for full functionality
3. **Timeout issues**: Increase timeout values in playwright.config.ts if needed
4. **Browser installation**: Run `npx playwright install` if browsers are missing

### Test Debugging

1. **Run in headed mode**:
```bash
npx playwright test --headed
```

2. **Run specific test**:
```bash
npx playwright test todo.spec.ts
```

3. **Debug mode**:
```bash
npx playwright test --debug
```

## 🔧 Customizing Tests

### Adding New Tests

1. Create new `.spec.ts` files in the `tests/` directory
2. Follow the existing pattern in `todo.spec.ts`
3. Use Playwright's rich API for element selection and assertions

### Modifying Existing Tests

The main test file `todo.spec.ts` includes:
- Creating todo items
- Deleting todo items
- Verifying UI state changes

### Test Data

Tests use UUID for unique test data to avoid conflicts between parallel runs.

## 📈 Best Practices

1. **Use data-testid attributes** for reliable element selection
2. **Wait for network idle** on page loads
3. **Clean up test data** after each test
4. **Use unique identifiers** to avoid test interference
5. **Take screenshots** on failure for debugging

## 🎯 Next Steps

1. **Extend test coverage** by adding more test scenarios
2. **Add visual regression testing** using Playwright's screenshot comparison
3. **Integrate with Azure DevOps** if using Azure DevOps for CI/CD
4. **Add performance testing** using Playwright's performance APIs
5. **Set up test data management** for more complex scenarios
