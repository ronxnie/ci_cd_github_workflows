# Intro GitHub Actions

This repository contains a sample project for learning and demonstrating GitHub Actions CI/CD workflows.

## Project Purpose

- Practice building GitHub Actions workflows
- Understand basic CI/CD pipeline concepts
- Learn how to connect a repository to automated build and test steps

## Contents

- `.github/workflows/` - Workflow definitions for GitHub Actions
- `.github/workflows/test-workflow.yaml` - A basic workflow that demonstrates a simple CI job
- `.github/workflows/conditional-workflow.yaml` - A multi-conditional workflow showing how steps can run based on branch and event conditions
- `.github/workflows/multi-job.yaml` - A multi-job workflow that runs several jobs and uses a failure-based condition
- `.github/workflows/environment_variable_git_variables_git_secret.yaml` - A workflow that demonstrates workflow-level environment variables, GitHub context variables, and the use of repository-level configuration values
- `hello.yaml` - A sample YAML file showing structured data and shell script examples
- `example.txt` - A simple text file used as a placeholder/example file
- `README.md` - Project overview and usage notes

## Workflow File Details

### 1. `test-workflow.yaml`

The file `.github/workflows/test-workflow.yaml` is a simple GitHub Actions workflow used to demonstrate automation.

- It is named `test-workflow`.
- It runs when the workflow is manually triggered using `workflow_dispatch` and when changes are pushed to the `main` branch.
- It defines a job called `test-job` that runs on `ubuntu-latest`.
- The job checks out the repository and runs a few example shell commands to simulate a test step.

### 2. `conditional-workflow.yaml`

The file `.github/workflows/conditional-workflow.yaml` demonstrates a multi-conditional workflow.

- It can be triggered manually with `workflow_dispatch` and on pushes to `main` or `develop`.
- It contains several steps that use `if` conditions to decide whether they should run.
- One step runs only on the `main` branch, another only on non-main branches, and others run only for certain event types.
- This is useful for showing how a workflow can behave differently depending on the branch, event, or input values.

### 3. `multi-job.yaml`

The file `.github/workflows/multi-job.yaml` demonstrates a multi-job workflow.

- It defines multiple jobs: `job1`, `job2`, and `job3`.
- `job1` and `job2` run independently on `ubuntu-latest`.
- `job3` uses the condition `if: failure()` so it runs only when a previous job has failed.
- This example shows how GitHub Actions can split work into separate jobs and manage dependencies or failure handling.

### 4. `environment_variable_git_variables_git_secret.yaml`

The file `.github/workflows/environment_variable_git_variables_git_secret.yaml` shows how to work with environment variables and GitHub context values inside a workflow.

- It defines workflow-level variables such as `APP_VERSION`, `APP_NAME`, and `ENVIRONMENT` under the top-level `env` section.
- It also sets a job-level environment variable named `APP_CLASS` for one job.
- It prints values using the `${{ env.VAR_NAME }}` syntax to access environment variables.
- It demonstrates built-in GitHub context values such as `${{ var.REPOSITORY }}`, `${{ var.REF }}`, `${{ var.SHA }}`, `${{ var.GITHUB_ACTOR }}`, and `${{ var.EVENT_NAME }}`.
- This workflow is a useful example for learning how configuration can be shared across jobs and how GitHub exposes repository and event metadata.

### 5. `hello.yaml`

The file `hello.yaml` is a sample YAML document used for practicing YAML syntax.

- It includes personal information in a structured format.
- It shows how nested objects and lists can be represented in YAML.
- It also contains example `script` and `run` blocks that demonstrate shell commands.

### 6. `example.txt`

The file `example.txt` is a simple text example file in the repository.

- It can be used as a placeholder or as a basic example for learning file handling.
- It is not a workflow file, but it helps keep the repository organized with sample content.

### Why workflow files are placed in `.github/workflows`

GitHub Actions looks for workflow definition files in the `.github/workflows` folder. Placing the files there ensures that GitHub recognizes them automatically and can run them based on the configured triggers.

## Repository-Level Variables and Secrets

You can create variables and secrets for the whole repository so they are available to all workflows.

### Create Repository Variables

1. Open your repository on GitHub.
2. Go to Settings > Secrets and variables > Actions.
3. Click the Variables tab.
4. Choose New repository variable.
5. Enter a name and value, for example:
   - `APP_NAME=my-app`
   - `APP_VERSION=1.0.0`
   - `ENVIRONMENT=production`
6. Save the variable.

Use repository variables in a workflow with the `vars` context:

```yaml
run: echo "App Name: ${{ vars.APP_NAME }}"
```

### Create Repository Secrets

1. Open your repository on GitHub.
2. Go to Settings > Secrets and variables > Actions.
3. Click the Secrets tab.
4. Choose New repository secret.
5. Enter a secret name and value.
6. Save the secret.

Use repository secrets in a workflow with the `secrets` context:

```yaml
run: echo "My secret is available"
env:
  MY_SECRET: ${{ secrets.MY_SECRET }}
```

> Repository variables are good for non-sensitive values, while secrets are meant for sensitive information such as passwords, tokens, or API keys.

## Usage

1. Open the repository in your code editor.
2. Review workflow files under `.github/workflows/`.
3. Update workflows as needed to fit your project and testing requirements.

## JSON vs YAML Format

Both JSON and YAML are used to represent structured data. They can describe the same information, but they differ in syntax and readability.

### JSON Format

- Uses curly braces `{}` to define objects
- Requires double quotes around keys and string values
- Uses commas to separate items
- Commonly used in APIs and web applications

Example:

```json
{
  "name": "Aditya",
  "age": 31,
  "languages": ["English", "Hindi", "Marathi", "Kannada", "Tulu"]
}
```

### YAML Format

- Uses indentation instead of braces
- Does not require quotes for simple values
- Uses hyphens for list items
- Is easier to read and write for humans
- Is better than JSON for configuration files because it is simpler and cleaner
- Supports nested structures very well, making it ideal for representing hierarchical data

Example:

```yaml
name: Aditya
age: 31
languages:
  - English
  - Hindi
  - Marathi
  - Kannada
  - Tulu
```

### Nested Structure Example

```yaml
person:
  - name: aditya
    address:
      city: Pune
      pincode: 412101
    phone:
      - type: mobile
        number: 1234567890
      - type: home
        number: 0987654321
  - name: Anusha
    address:
      city: Pune
      pincode: 412101
    phone:
      - type: mobile
        number: 9876543210
      - type: home
        number: 0123456789
  - name: Ananya
    address:
      city: Mumbai
      pincode: 400001
    phone:
      - type: mobile
        number: 5678901234
      - type: home
        number: 4321098765
```

### Comparison

- JSON is stricter and more widely used in software integrations.
- YAML is more human-friendly and easier to write for configuration purposes.
- YAML is often considered better than JSON for config files because of its simplicity and readability.
- Both can represent the same data structure, and YAML handles nested structures more clearly.

## Script and Run Commands

The YAML example also includes a simple script block and a run block. These are commonly used to define commands that should be executed in a shell.

### Script Block

The `script` section prints a few example messages to the terminal:

```yaml
script: |
  echo "Hello, World!"
  echo "This is a sample script in YAML format."
  echo "YAML is a human-readable data serialization format."
```

### Run Block

The `run` section demonstrates basic shell commands such as changing directories, listing files, and displaying the current working directory:

```yaml
run: |
  cd /home/user
  ls -l
  pwd
  echo "Current directory: $(pwd)"
```

These examples help show how YAML can be used to describe automation steps in a clean and readable format.

### Purpose of the Pipeline Operator `|`

The pipeline operator `|` in YAML is used to create a block scalar, which allows multi-line text to be written as a single value. It is especially useful for script content, shell commands, and long text blocks because it preserves line breaks and formatting.

Example:

```yaml
script: |
  echo "Hello"
  echo "World"
```

This makes the YAML file easier to read and helps define commands clearly for automation tools such as GitHub Actions.

## Git vs GitHub vs GitHub Actions

The following table highlights the main differences between Git, GitHub, and GitHub Actions:

| Topic | Git | GitHub | GitHub Actions |
|------|-----|--------|----------------|
| Purpose | A version control system used to track changes in code | A cloud-based platform for hosting Git repositories and collaboration | A CI/CD service for automating workflows in repositories |
| Main Function | Manages code history, branching, and commits | Provides repository hosting, pull requests, issues, and team collaboration | Runs automated build, test, and deployment workflows |
| Usage | Used locally on your machine | Used online for sharing and managing projects | Used to automate software development processes |
| Example | `git commit`, `git push` | Create repositories, review code, manage pull requests | Build, test, and deploy code after every push |
| Focus | Code versioning | Collaboration and repository hosting | Automation and DevOps pipelines |

## Introduction to GitHub Actions Workflow

A GitHub Actions workflow is a set of automated instructions that run when certain events happen in a repository, such as a push, pull request, or release. A workflow can contain one or more jobs, and each job can perform tasks like installing dependencies, running tests, building code, or deploying applications.

This makes GitHub Actions useful for automating repetitive development tasks and ensuring that your project works correctly every time changes are made.

## Create a GitHub Repository and Runner

Follow these steps to create a GitHub repository and set up a runner for automation.

### 1. Create a GitHub Repository

1. Sign in to GitHub.
2. Click the New repository button.
3. Enter a repository name and description.
4. Choose whether the repository should be public or private.
5. Click Create repository.

### 2. Connect Your Local Project

1. Open your local project folder in the terminal.
2. Run the following commands:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <your-repository-url>
git push -u origin main
```

### 3. Create a GitHub Actions Runner

1. Open the repository on GitHub.
2. Go to Settings > Actions > Runners.
3. Click New self-hosted runner.
4. Choose your operating system and follow the installation steps.
5. Download and configure the runner on your machine.
6. Start the runner using the provided command.

### 4. Use the Runner in a Workflow

Once the runner is set up, create a workflow file in `.github/workflows/` to run jobs automatically on push or pull request events.

Example:

```yaml
name: CI
on: [push]
jobs:
  build:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v4
      - run: echo "Hello from GitHub Actions"
```

This allows your repository to automate tasks such as building, testing, and deploying code.

## Explanation of the Runner Workflow YAML File

A GitHub Actions workflow YAML file defines when and how automation should run. It usually contains the following main parameters:

- `name`: Gives a friendly name to the workflow.
- `on`: Specifies the event that triggers the workflow, such as `push`, `pull_request`, or `workflow_dispatch`.
- `jobs`: Defines the tasks or steps to be executed.
- `runs-on`: Selects the runner environment. For example, `ubuntu-latest` uses a GitHub-hosted Ubuntu machine.
- `steps`: Lists the actions or shell commands that should run inside the job.

### Installing Python

Python is often used in CI/CD pipelines for scripts, automation, and testing. To use Python in GitHub Actions, it is helpful to install it on your local machine first.

#### On Windows

1. Download Python from the official Python website.
2. Run the installer and check the option to add Python to `PATH`.
3. Verify the installation:

```bash
python --version
pip --version
```

#### On Ubuntu / WSL

```bash
sudo apt update
sudo apt install python3 python3-pip -y
python3 --version
pip3 --version
```

#### Additional Points

- Use a virtual environment for project-specific dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
```

- On Windows, activate the virtual environment with:

```bash
.venv\Scripts\activate
```

- Install packages with `pip` when needed:

```bash
pip install requests
```

### GitHub Marketplace

GitHub Marketplace is a place where developers can discover and use reusable GitHub Actions, apps, and automation tools created by the community and official vendors.

- It helps you add features like testing, deployment, notifications, and code quality checks quickly.
- You can browse actions by category and install them in your repository.
- Popular examples include actions for checkout, Node.js setup, Python setup, Docker, and deployment.

### Differences in Versions

When working with GitHub Actions, you may see versions such as `v4`, `v5`, or `v6` in action references.

- A major version like `@v4` usually points to the latest compatible release within that major line.
- A more specific version such as `@v4.1.0` or `@v6.1.2` is more precise and may include bug fixes or new features.
- It is good practice to use a stable version pin to avoid unexpected changes in behavior.

Example:

```yaml
steps:
  - uses: actions/checkout@v6
```

### When `runs-on` is set to `ubuntu-latest`

If `runs-on: ubuntu-latest` is used, GitHub Actions will execute the workflow on a GitHub-hosted Ubuntu runner. This is useful when you want a ready-to-use environment without managing your own machine.

Example:

```yaml
name: Example Workflow
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Running on Ubuntu"
```

### Important Points to Maintain While Creating the File

1. Use valid YAML indentation.
2. Make sure the workflow triggers are correctly defined under `on`.
3. Keep the `jobs` structure clear and organized.
4. Choose the correct runner type, such as `ubuntu-latest`, `windows-latest`, or `self-hosted`.
5. Ensure each step has the proper commands or actions.

This helps the workflow run smoothly and makes the automation easier to understand and maintain.

## Notes

- This repository is intended as an introductory example for GitHub Actions.
- Add additional documentation, scripts, and pipeline details as the project evolves.
