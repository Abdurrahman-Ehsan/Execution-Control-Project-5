
What is the if: condition?
Normally, GitHub Actions executes steps sequentially.

For example:
steps:
  - name: Install dependencies
    run: npm ci

  - name: Run tests
    run: npm test

  - name: Build application
    run: npm run build


Suppose the Run tests step fails.

By default:
- GitHub marks that step as failed.
- Subsequent steps that depend on normal success are skipped.
- The job is marked as failed.
But what if you want to execute another step specifically when the tests fail?

That's where if: becomes useful.

What is the failure() function?
failure() is a GitHub Actions status-check function.

It returns:
- true when a previous step in the job has failed, or a previous job in the dependency chain has failed.
- false when no such failure has occurred.
You can use it in an if: condition.
Example: Execute a step after failure
steps:
  - name: Run tests
    run: npm test

  - name: Handle test failure
    if: failure()
    run: echo "The tests have failed!"


    In GitHub Actions, continue-on-error is a setting that allows a workflow to continue even when a particular step or job fails.

Difference between outcome and conclusion
Suppose the test step fails but has continue-on-error: true.
Property	Value
steps.test.outcome	failure
steps.test.conclusion	success

In GitHub Actions, a Matrix strategy allows you to run the same job multiple times with different configurations, without writing that job repeatedly.

For example, suppose you want to test your application using three Node.js versions:
- Node.js 18
- Node.js 20
- Node.js 22
Instead of creating three separate jobs, you can use a matrix.
1. Basic example
name: Matrix Example

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18, 20, 22]

    steps:
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Run tests
        run: npm test