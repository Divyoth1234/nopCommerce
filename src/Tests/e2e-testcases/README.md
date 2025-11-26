# Test Automation with testRigor for nopCommerce

This document provides step-by-step instructions for setting up end-to-end (E2E) test automation for **nopCommerce** using **testRigor**.
It covers creating a testRigor account, setting up a test suite, and running tests via GitHub Actions and the testRigor CLI.

## Creating an Account on testRigor

1. **Visit the testRigor website:**

   - Go to [testRigor](https://www.testrigor.com/).

2. **Sign up for a new account:**

   - Click on the "Sign Up" button on the top right corner.
   - Select the "Public Open Source" version.
   - Fill in the required details and follow the instructions to complete the registration.

3. **Verify your email and log in:**

   - Check your email inbox for a verification email from testRigor.
   - Click on the verification link to activate your account.
   - Once your account is activated, log in.

4. **Create a test sute:**
   - After logging into your account, create a test suite.
  
## How It Works (CI via GitHub Actions)

An automated GitHub Actions workflow located at: .github/workflows/nopcommerce-testrigor.yml
performs the following steps:

- Checks out the repository
- Sets up the .NET 8 runtime (required for nopCommerce)
- Starts Microsoft SQL Server 2022 as a Docker service
- Creates the nopCommerce database and database user
- Downloads and runs the nopCommerce **NoSource Linux** package
- Waits for nopCommerce to be available at: http://127.0.0.1:5000
- Builds test data (`variables.json`) from stored values and GitHub Secrets
- Runs the testRigor test suite via the testRigor CLI
- Loads test cases and reusable rules directly from this repository

### Test Data Handling
- `storedValues.yaml` contains **placeholders only**
- Sensitive values (email/password) are injected securely via GitHub Secrets
- A CLI-compatible `variables.json` file is generated at runtime

## Running Tests with the CLI (Optional)

> Local execution is optional.  
> CI via GitHub Actions is the primary execution path.  
> These steps assume nopCommerce is already running locally at `http://127.0.0.1:5000`.


1. **Install testRigor CLI**
 ```bash
 npm install -g testrigor-cli
 ```

2. **Run Tests:**
 ```bash
   testrigor test-suite run <SUITE_ID> \
  --token <CI_TOKEN> \
  --localhost \
  --test-cases-path src/Tests/e2e-testcases/testcases/**/*.yaml \
  --rules-path src/Tests/e2e-testcases/reusable-rules/**/*.yaml \
  --variables-path src/Tests/e2e-testcases/testdata/variables.json \
  --url "http://127.0.0.1:5000"
 ```

3. **View Test Results:**
   - You can view the results on testRigor by opening the link shown in the terminal.


## Configuration 
Set these GitHub repository secrets/variables:

### Secrets:
- TESTRIGOR_CICD_TOKEN: Your testRigor authentication token (from CI/CD Integration page)
- TR_CUSTOMER_EMAIL: Customer email used in tests
- TR_CUSTOMER_PASS: Customer password used in tests
- MSSQL_SA_PASSWORD: SQL Server SA password
- NOP_USER_PASSWORD: nopCommerce database user password

### Variables:
- TESTRIGOR_SUITE_ID: The testRigor test suite identifier

## Learn More
- [testRigor Documentation](https://testrigor.com/docs/)
- [testRigor CLI Reference](https://testrigor.com/command-line)
