
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