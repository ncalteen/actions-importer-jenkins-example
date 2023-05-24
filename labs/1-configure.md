# Configure Credentials for GitHub Actions Importer

In this lab, you will use the `configure` CLI command to set the required
credentials and information for GitHub Actions Importer to use when working with
Jenkins and GitHub.

## Configure Credentials

### Step 1: Create a Jenkins API Token

1. Open the [Jenkins server](http://localhost:8080) in a new browser tab
2. In the top right menu bar, click the `admin` button
3. In the left panel, click the `Configure` gear
4. In the `API token` section, click the `Add new Token` button
5. Click `Generate`
6. Copy the generated API token and save it in a safe location

### Step 2: Create a GitHub Personal Access Token (PAT)

1. Open [`github.com`](https://github.com) in a new browser tab
2. In the top right corner of the UI, click your profile photo and click
   `Settings`
3. In the left panel, click `Developer Settings`
4. Click `Personal access tokens` and then `Tokens (classic)`
5. Click `Generate new token` and then `Generate new token (classic)`

   You may be required to authenticate with GitHub during this step

6. Name your token in the `Note` field
7. Select the following scope: `workflow`
8. Click `Generate token`
9. Copy the generated PAT and save it in a safe location

### Step 3: Run the `configure` CLI Command

1. Open a new terminal window
2. Run the following command

   ```bash
   gh actions-importer configure
   ```

3. Use the down arrow key to highlight `Jenkins`
4. Press the spacebar to select
5. Press enter to continue
6. At the GitHub handle prompt, enter your GitHub handle
7. At the GitHub PAT prompts, enter the GitHub PAT generated in
   [step 2](#step-2-create-a-github-personal-access-token-pat) and press enter
8. At the GitHub URL prompt, enter the GitHub instance URL or press enter to
   accept the default value (`https://github.com`)
9. At the Jenkins token prompt, enter the Jenkins access token from
   [step 1](#step-1-create-a-jenkins-api-token) and press enter
10. At the Jenkins username prompt, enter `admin` and press enter
11. At the Jenkins URL prompt, enter `http://localhost:8080/` and press enter

```bash
$ gh actions-importer configure
✔ Which CI providers are you configuring?: Jenkins
Enter the following values (leave empty to omit):
✔ GitHub handle used to authenticate with the GitHub Container Registry: ncalteen
✔ Personal access token to authenticate with the GitHub Container Registry: ****************************************
✔ Personal access token for GitHub: ****************************************
✔ Base url of the GitHub instance: https://github.com
✔ Personal access token for Jenkins: **********************************
✔ Username of Jenkins user: admin
✔ Base url of the Jenkins instance: http://localhost:8080/
Environment variables successfully updated.
```

## Verify your Environment

To verify your environment is configured correctly, run the `update` CLI
command. The `update` CLI command will download the latest version of GitHub
Actions Importer to your codespace.

### Step 1: Run the `update` CLI Command

1. Open a new terminal window
2. Run the following command

   ```bash
   gh actions-importer update
   ```

You should see a confirmation that you were logged into the GitHub Container
Registry and the image was updated to the latest version.

```bash
$ gh actions-importer update
Login Succeeded

Updating ghcr.io/actions-importer/cli:latest...
ghcr.io/actions-importer/cli:latest up-to-date
```

### Next lab

[Perform an Audit of a Jenkins Server](2-audit.md)
