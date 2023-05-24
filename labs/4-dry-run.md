# Perform a Dry-Run Migration of a Jenkins Pipeline

In this lab you will use the `dry-run` command to convert a Jenkins pipeline to
its equivalent GitHub Actions workflow.

## Step 1: Perform a Dry Run

When running a dry-run, the following inputs must be provided:

| Option         | Description                        | Example                                   |
| -------------- | ---------------------------------- | ----------------------------------------- |
| `--output-dir` | Output path for logs and artifacts | `tmp/dry-run`                             |
| `--source-url` | URL to the source Jenkins pipeline | `http://localhost:8080/job/test_pipeline` |

1. Open a new terminal window
2. Run the following command

   ```bash
   gh actions-importer dry-run jenkins --source-url http://localhost:8080/job/test_pipeline --output-dir tmp/dry-run
   ```

The command will list all the files written to disk when the command succeeds.

```bash
$ gh actions-importer dry-run jenkins --source-url http://localhost:8080/job/test_pipeline --output-dir tmp/dry-run
[2022-09-28 20:12:00] Logs: 'tmp/dry-run/log/actions-importer-20220928-201200.log'
[2022-09-28 20:12:00] Output file(s):
[2022-09-28 20:12:00]   tmp/dry-run/test_pipeline/.github/workflows/test_pipeline.yml
```

## Step 2: Inspect the Output Files

The files generated from the `dry-run` command represent the equivalent Actions
workflow for the given Jenkins pipeline. For example, the following code blocks
show the initial Jenkins pipeline and the converted GitHub Actions workflow.

### Jenkins Pipeline

```groovy
pipeline {
  agent {
    label 'TeamARunner'
  }

  environment {
    DISABLE_AUTH = 'true'
    DB_ENGINE    = 'sqlite'
  }

  stages {
    stage('build') {
      steps {
        echo "Database engine is ${DB_ENGINE}"
        sleep 80
        echo "DISABLE_AUTH is ${DISABLE_AUTH}"
      }
    }
    stage('test') {
      steps{
        junit '**/target/*.xml'
      }
    }
  }
}
```

### GitHub Actions Workflow

```yaml
name: test_pipeline

on:
  push:
    paths: '*'
  schedule:
    - cron: 0-29/10 * * * *

env:
  DISABLE_AUTH: 'true'
  DB_ENGINE: sqlite

jobs:
  build:
    runs-on:
      - self-hosted
      - TeamARunner

    steps:
      - name: checkout
        uses: actions/checkout@v3.5.0

      - name: echo message
        run: echo "Database engine is ${{ env.DB_ENGINE }}"
      #     # This item has no matching transformer
      #     - sleep:
      #       - key: time
      #         value:
      #           isLiteral: true
      #           value: 80

      - name: echo message
        run: echo "DISABLE_AUTH is ${{ env.DISABLE_AUTH }}"
  test:
    runs-on:
      - self-hosted
      - TeamARunner
    needs: build

    steps:
      - name: checkout
        uses: actions/checkout@v3.5.0

      - name: Publish test results
        uses: EnricoMi/publish-unit-test-result-action@v2.7.0
        if: always()
        with:
          junit_files: '**/target/*.xml'
```

### Summary

These two pipelines function equivalently despite using different syntax. In
this case, the pipeline conversion was “partially successful” (that is, some
items were not automatically converted) and the unconverted item was placed as
comment in the location the Jenkins pipeline used it.

```diff
- sleep 80
+ #     # This item has no matching transformer
+ #     - sleep:
+ #       - key: time
+ #         value:
+ #           isLiteral: true
+ #           value: 80
```

In the next lab, you'll learn how to override GitHub Actions Importer's default
behavior and customize the converted workflow that is generated.

Try running the `dry-run` command for different pipelines in the Jenkins server.
As a hint, you only have to change the `--source-url` CLI option.

## Next lab

[Customize GitHub Actions Importer's Behavior with Custom Transformers](5-custom-transformers.md)
