# Forecast Potential Runner Usage

In this lab you will use the `forecast` command to forecast potential GitHub
Actions usage by computing metrics from completed pipeline runs in your Jenkins
server.

## Step 1: Perform a Forecast

When running a forecast, the following inputs must be provided:

| Option         | Description                        | Example        |
| -------------- | ---------------------------------- | -------------- |
| `--output-dir` | Output path for logs and artifacts | `tmp/forecast` |
| `--start-date` | Start date to forecast from        | `2022-08-02`   |

1. Open a new terminal window
2. Run the following command

   ```bash
   gh actions-importer forecast jenkins --output-dir tmp/forecast --start-date 2022-08-02
   ```

   > **Note:** In this example you will forecast the entire Jenkins instance,
   > but in the future if you wanted to forecast a specific folder, add the
   > `-f <folder_path>` flag to the command.

The command will list all the files written to disk when the command succeeds.

```bash
$ gh actions-importer forecast jenkins --output-dir tmp/forecast --start-date 2022-08-02
[2022-08-20 22:08:20] Logs: 'tmp/forecast/log/actions-importer-20220916-021004.log'
[2022-08-20 22:08:20] Forecasting 'http://localhost:8080/'
[2022-08-20 22:08:20] Output file(s):
[2022-08-20 22:08:20]   tmp/forecast/jobs/09-16-2022-02-10_jobs_0.json
[2022-08-20 22:08:20]   tmp/forecast/forecast_report.md
```

## Step 2: Review the Forecast Report

The forecast report, logs, and completed job data will be located within the
`tmp/forecast` folder. This file contains metrics used to forecast potential
GitHub Actions usage.

1. Open the `forecast_report.md` file

### Total Section

The "Total" section of the forecast report contains high level statistics
related to all the jobs completed after the date specified in the `--start-date`
CLI option. Additionally, these metrics are defined for each queue of runners
defined in Jenkins. This is especially useful if there are a mix of
hosted/self-hosted runners or high/low spec machines to see metrics specific to
different types of runners.

```md
## Total

- Job count: **73**
- Pipeline count: **6**

- Execution time

  - Total: **8,555 minutes**
  - Median: **2 minutes**
  - P90: **17 minutes**
  - Min: **0 minutes**
  - Max: **4,072 minutes**

- Queue time

  - Median: **0 minutes**
  - P90: **0 minutes**
  - Min: **0 minutes**
  - Max: **0 minutes**

- Concurrent jobs

  - Median: **0**
  - P90: **0**
  - Min: **0**
  - Max: **29**
```

**Key terms:**

- The **Job count** is the total number of completed jobs.
- The **Pipeline count** is the number of unique pipelines used.
- **Execution time** describes the amount of time a runner spent on a job. This
  metric can be used to help plan for the cost of GitHub-hosted runners.
  - This metric is correlated to how much you should expect to spend in GitHub
    Actions. This will vary depending on the hardware used for these minutes.
    You can use the
    [Actions pricing calculator](https://github.com/pricing/calculator) to
    estimate a dollar amount.
- **Queue time** metrics describe the amount of time a job spent waiting for a
  runner to be available to execute it.
- **Concurrent jobs** metrics describe the amount of jobs running at any given
  time. This metric can be used to define the number of runners a customer
  should configure.

## Next steps

[Perform a Dry-Run Migration of a Jenkins Pipeline](4-dry-run.md)
