# Migrating from Jenkins using GitHub Actions Importer

These instructions will guide you through configuring a GitHub Codespaces
environment that you will use in subsequent labs to learn how to use GitHub
Actions Importer to migrate Jenkins pipelines to GitHub Actions.

## Bootstrap a Jenkins server

1. Execute the Jenkins setup script that will start a container with a Jenkins
   server running inside of it.

   - Run the following command from the workspace root to start a Jenkins
     server:

     ```bash
     $ ./bootstrap/setup.sh
     Jenkins is up and running!

     URL: http://localhost:8080
     Username: admin
     Password: password
     ```

2. Open the Jenkins server in your browser and use the following credentials to
   authenticate:

   - Username: `admin`
   - Password: `password`

3. Once authenticated, you should see a Jenkins server with a few predefined
   pipelines.

## Jenkins Labs

Perform the following labs to learn more about Actions migrations with GitHub
Actions Importer:

1. [Configure credentials for GitHub Actions Importer](labs/1-configure.md)
2. [Perform an audit of a Jenkins server](labs/2-audit.md)
3. [Forecast potential build runner usage](labs/3-forecast.md)
4. [Perform a dry-run migration of a Jenkins pipeline](labs/4-dry-run.md)
5. [Use custom transformers to customize GitHub Actions Importer's behavior](labs/5-custom-transformers.md)
6. [Perform a production migration of a Jenkins pipeline](labs/6-migrate.md)

## About GitHub Actions Importer (GAI)

The GitHub Actions Importer (GAI) is a [GitHub CLI](https://cli.github.com/)
extension to plan, forecast, and automate migrations from Jenkins to GitHub
Actions. Each of the phases of the migration process are described below.

1. **Planning:** Analyze existing CI/CD usage to build a roadmap for migration.
   The output of this phase will be a prioritized list of pipelines to migrate
   based on business need, complexity, effort, and risk.
2. **Testing:** Conduct dry-run migrations to validate the converted workflows.
   GAI supports repeated testing to ensure custom behavior is preserved.
3. **Migration:** Generate validated workflows as pull requests into your target
   repository. Review any constructs that could not be migrated automatically.

Instructions to install GAI can be found
[here](https://docs.github.com/en/actions/migrating-to-github-actions/automated-migrations/automating-migration-with-github-actions-importer).

GitHub's published goal is an 80% conversion rate for every workflow. However,
this can vary based on considerations such as the following:

- Scripted pipelines are not currently supported
- Mandatory build tools must be migrated manually
- Secrets must be migrated manually
- Self-hosted runners must be migrated manually
- Unknown plugins must be migrated manually

## About Workflow Files

When GAI converts a Jenkins pipeline to a GitHub Actions workflow file, there
are several key items to review.

### Custom Transformers

For anything that GitHub Actions Importer was not able to convert automatically,
such as unknown build steps or a partially successful pipeline, you might want
to create custom transformers to further customize the conversion process. For
more information, see
[Extending GitHub Actions Importer with custom transformers](https://docs.github.com/en/actions/migrating-to-github-actions/automated-migrations/extending-github-actions-importer-with-custom-transformers).

### Workflow Comments

If any portion of your pipeline was not successfully converted, it displays as a
comment in the same location the Jenkins pipeline used it. For example, the
following snippet shows a Jenkinsfile that uses a Docker agent.

```groovy
pipeline {
  agent {
    docker {
      image 'node:18.16.0-alpine'
      args '-u root:root'
    }
  }
  //...
}
```

When converting to a workflow file, the `container` definition will include a
comment for the container arguments, as there is no matching transformer.

```yaml
Check_Node:
  name: Check Node
  runs-on: ubuntu-latest
  container:
    image: node:18.16.0-alpine
#       # This item has no matching transformer
#       docker:
#         key: args
#         value:
#           isLiteral: true
#           value: "-u root:root"
```

This can be resolved by converting the commented section to an `options`
property in the `container` definition.

```yaml
Check_Node:
  name: Check Node
  runs-on: ubuntu-latest
  container:
    image: node:18.16.0-alpine
    options: '-u root:root'
```

### Actions Equivalents

In some cases, certain build steps or commands are replaced with an equivalent
Action from the GitHub Marketplace. These actions perform the same task as the
original build step. When running the `audit` command, the output file will list
any actions that were used in the converted workflows. If you wish to use these
actions, you will need to ensure they are allowed by your GitHub organization's
settings. For example, the `Checkout SCM` build step is automatically replaced
with the [`actions/checkout`](https://github.com/actions/checkout) action.

## FAQ

### What plugin(s) are supported by GAI?

For a list of supported plugins, see the
[`github/gh-actions-importer`](https://github.com/github/gh-actions-importer/blob/main/docs/jenkins/index.md)
repository.

### What pipeline syntax is supported by GAI?

See
[Supported syntax for Jenkins pipelines](https://docs.github.com/en/actions/migrating-to-github-actions/automated-migrations/migrating-from-jenkins-with-github-actions-importer#supported-syntax-for-jenkins-pipelines).
