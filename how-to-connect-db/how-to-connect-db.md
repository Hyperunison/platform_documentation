# Document Overview

This document contains step-by-step instructions on how to connect your database to the Unison platform and start working. Follow these instructions, and if you have any questions, feel free to message us on Slack. [Click here](https://join.slack.com/t/unisoncommunity/shared_invite/zt-2p75t4fhc-Kiksdz2sf19zjYi_JVHzNg) to join the Unison Slack community.

# Instructions

## Create a Data Repository

1. Navigate to the **Converter** section in the main menu.
2. Click the **ADD EXISTING REPOSITORY** button in the top-right corner of the screen.  
   - *The repository creation screen will open.*
3. Enter the repository name.
4. Enter a unique 6-character code.
5. Click **Create**.  
   - *You will be redirected to the "Data Repositories" page, displaying a list of repositories.*
6. Click the **RUNNERS** button.
7. Click **ADD RUNNER**.  
   - *A runner will be created in the Unison platform.*
8. Click the three dots button and select **Copy Token**.

![GIF](./media/create-repository.gif)

---

## Download the Runner to Your Working Environment

1. Click the **How to Install** button.  
   - *This will open the GitHub repository.*
2. Click the green **Code** button and download the `.zip` file.
3. Upload the runner to the environment that has database access.
4. Extract the `.zip` file.

![GIF](./media/download-runner.gif)

### Requirements for the Environment

- Docker version 24+
- Network access to `app.hyperunison.com`
- Network access to the source database

---

## Set Up the Runner

The configuration file is `config.yaml`. An example configuration file with comments can be found [here](./yaml.md).

1. Open the extracted folder with your IDE.
2. Find the `config-dist.yaml` file and rename it to `config.yaml`.
3. Open `config.yaml` in your IDE or text editor.
4. Locate the **agent_token** setting and paste the token copied from step 8 in the "Create a Data Repository" section.  
   - *Example:*  
     agent_token: "05300f8d64ed26d45acf0cb13cb23120"
5. Locate the **dsn** setting and configure it for your database:  
   - *Example:*  
     dsn: postgresql+psycopg2://ukdb:7boGiq93ybdoTY3gofIGdieg@185.175.46.57:5433/allergies

For editing, you can use any IDE, Notepad++, or a similar text editor.  
An example configuration file with comments is available [here](https://github.com/Hyperunison/Runner/blob/main/config-dist.yaml).

---

## Scan the Database

Start the runner and ensure it runs continuously, as it listens for commands from the platform.

### Using Docker-Compose
1. Copy `config-dist.yaml` to `config.yaml`.
2. Update `config.yaml` to match your setup. All properties are described in the `config.yaml` file.
3. Run the following command:  
   `docker-compose up --build`
   *Logs will appear in your terminal.*

### AWS
1. Generate credentials and save them in the `Resources/.aws` folder.  
   - The folder must contain two files: `config` and `credentials`.

### Kubernetes (k8s)
1. If you use Kubernetes with AWS, follow the AWS setup instructions.
2. Copy the Kubernetes configuration file to the `Resources/.kube` folder.  
   - The folder must contain a single file named `config`.
3. More information about Kubernetes configuration files can be found [here](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/).

---

### How to Identify When Scanning Starts and Finishes

- **Scanning has started:**  
  The list of source tables will appear in the Structure Mapping interface.

- **Scanning has finished:**  
  You will see a log message with `log.level < 10`, like the following example:  
  2024-11-04 11:15:31,947 [DEBUG] [main] response body: b'{\n    "type": "idle",\n    "readCount": 0,\n    "data": {\n        "idle": true\n    },\n    "status": "none"\n}'

---

## Check Results on the Platform

1. Open the **Converter** section.
2. Find the dataset you added and open the **Structure Mapping** interface.
3. Use the **Source Explorer** section to review the values.

![GIF](./media/check-scan-results.gif)