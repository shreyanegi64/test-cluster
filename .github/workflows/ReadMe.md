# CI/CD Pipeline

This repository contains a GitHub Actions workflow for a CI/CD pipeline designed to automate various tasks, including building and deploying Docker containers, managing Kubernetes resources, and setting environment-specific API keys based on the commit author.

## Workflow Overview

The workflow is triggered on any push to the `main` branch. It includes the following jobs and steps:

### Job: `check_files`

#### Steps:

1. **Checkout code**:
   - Uses the `actions/checkout@v3` action to check out the repository code.
   - Fetches the latest 2 commits to ensure sufficient commit history.

2. **Set up Docker Buildx**:
   - Uses the `docker/setup-buildx-action@v1` action to set up Docker Buildx.

3. **Login to Docker Hub**:
   - Logs in to Docker Hub using credentials stored in GitHub secrets (`DOCKER_USERNAME_DATAOS` and `DOCKER_PASSWORD_DATAOS`).

4. **Pull Docker image**:
   - Pulls the specified Docker image (`rubiklabs/dataos-ctl:2.10.9-dev`).

5. **Run Docker container**:
   - Runs the pulled Docker container.

6. **Get commit author**:
   - Retrieves the author of the most recent commit and sets it as an environment variable.

7. **Set API Key and User ID**:
   - Sets environment variables `USER_ID` and `API_KEY` based on the commit author using credentials stored in GitHub secrets.

8. **List changed files**:
   - Lists the files changed in the most recent commit and saves them to a file named `changed_files.txt`.

9. **Delete resources**:
   - Deletes Kubernetes resources defined in the changed YAML files using the `rubiklabs/dataos-ctl` Docker image.

10. **Check delete result**:
    - Retries deleting the resources if the initial delete attempt fails, with a 30-second wait period.

11. **Apply configuration**:
    - Applies the configuration defined in the changed YAML files using the `rubiklabs/dataos-ctl` Docker image with linting enabled.

12. **Apply configuration without linter**:
    - Applies the configuration defined in the changed YAML files using the `rubiklabs/dataos-ctl` Docker image without linting.

## Environment Variables and Secrets

The workflow uses several environment variables and GitHub secrets to manage credentials and configuration:

- `DATAOS_PRIME_APIKEY`: API key for DataOS.
- `LICENSE_KEY`: License key for the application.
- `LICENSE_ORGANIZATION_ID`: License organization ID.
- `DOCKER_USERNAME_DATAOS`: Docker Hub username for DataOS.
- `DOCKER_PASSWORD_DATAOS`: Docker Hub password for DataOS.
- `SHREYA_USERNAME`, `SHREYA_API_KEY`: Credentials for user "shreya negi".
- `DEEPAK_USERNAME`, `DEEPAK_API_KEY`: Credentials for user "itsyourdj".
- `KHUSHI_USERNAME`: Username for user "kanak_gupta" (shares the same API key as "itsyourdj").
- `CHENNA_USERNAME`, `CHENNA_API_KEY`: Credentials for user "chenna_kesava".
- `MONEESH_USERNAME`, `MONEESH_API_KEY`: Credentials for user "moneeshtmdc".

## Usage

To use this workflow, ensure that you have configured the necessary GitHub secrets in your repository settings. These secrets include the Docker Hub credentials, API keys, and user-specific credentials.

The workflow will automatically run on any push to the `main` branch. It will:

1. Check out the code.
2. Set up Docker Buildx.
3. Log in to Docker Hub.
4. Pull and run the specified Docker image.
5. Determine the commit author and set environment variables accordingly.
6. List the changed YAML files.
7. Delete the corresponding DataOS resources.
8. Retry deletion if it fails initially.
9. Apply the new configuration using the changed YAML files.
10. Apply the configuration again without linting.

## Note

- The workflow assumes that the necessary Docker images and Kubernetes configurations are properly set up.
- Ensure that the specified Docker image (`rubiklabs/dataos-ctl:2.10.9-dev`) is available on Docker Hub.






