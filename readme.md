# Jenkins Migrations Powered by GitHub Actions Importer (GAI)

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

## Labs for Jenkins

Perform the following labs to learn more about Actions migrations with GitHub
Actions Importer:

1. [Configure credentials for GitHub Actions Importer](labs/1-configure.md)
2. [Perform an audit of a Jenkins server](labs/2-audit.md)
3. [Forecast potential build runner usage](labs/3-forecast.md)
4. [Perform a dry-run migration of a Jenkins pipeline](labs/4-dry-run.md)
5. [Use custom transformers to customize GitHub Actions Importer's behavior](labs/5-custom-transformers.md)
6. [Perform a production migration of a Jenkins pipeline](labs/6-migrate.md)
