# Document Overview
This document contains step-by-step instructions on how to connect your database to the Unison platform and start working. Follow it and message us on Slack if you have any questions. [Click here](https://join.slack.com/t/unisoncommunity/shared_invite/zt-2p75t4fhc-Kiksdz2sf19zjYi_JVHzNg) to join the Unison Slack community.

# Instructions

## Create a Data Repository
1. Go to the converter section from the main menu.
2. Click on the "ADD EXISTING REPOSITORY" button in the top right corner of the screen.
   - *The system will open the repository creation screen.*
3. Enter the name of the repository.
4. Enter a 6-symbol unique code.
5. Click "Create".
   - *The system will open the "Data Repositories" page with the list of repositories.*
6. Click on the "RUNNERS" button.
7. Click on "ADD RUNNER".
   - *The system will create a runner in the Unison platform.*
8. Click on the three dots button, then click "Copy Token".

![GIF](./media/create-repository.gif)

## Download the Runner to Your Working Environment
1. Click on the "How to Install" button.
   - *The GitHub repository will open.*
2. Click on the green "Code" button and download the .zip file.
3. Upload the runner to the environment that has access to the database.
4. Extract the .zip file.

![GIF](./media/download-runner.gif)

### Requirements for the Environment
- Docker version 24+
- Network access to `app.hyperunison.com`
- Network access to the source database

## Set Up the Runner
The configuration file is `config.yaml`. Example of the file with comments you can get [here](./yaml.md).

1. Open the extracted folder with your IDE.
2. Find `config-dist.yaml` and rename it by removing "-dist" from the filename.
3. Open the configuration file.
4. Find the setting **agent_token** and copy the token from the runner on the platform (refer to point 8 from the "Create Data Repository" section).
   - *The value should look like:* `agent_token: "05300f8d64ed26d45acf0cb13cb23120"`
5. Find the setting **dsn**:
   - *It should look like:* `dsn: postgresql+psycopg2://ukdb:7boGiq93ybdoTY3gofIGdieg@185.175.46.57:5433/allergies`

You can edit it using an IDE or tools like Notepad++ or any text editor.

Example of the file with comments you can get [here](https://github.com/Hyperunison/Runner/blob/main/config-dist.yaml). 

## Scan the Database
You need to start the runner and keep it running continuously, as it listens for commands from the platform.

### Using Docker-Compose
1. Copy `config-dist.yaml` to `config.yaml`.
2. Update `config.yaml` to suit your requirements. Properties are described in the `config.yaml`.
3. Run:
   ```bash
   docker-compose up --build
   ```
   - *After this, you will see logs in your terminal.*

### AWS
1. Generate credentials and place them in the `Resources/.aws` folder. The folder should contain two files: `config` and `credentials`.

### Kubernetes (k8s)
1. If you use Kubernetes inside AWS, follow the AWS instructions.
2. Copy the Kubernetes config to the `Resources/.kube` folder. The folder should contain a single file named `config`.
3. More information about the Kubernetes config file can be found [here](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/).

### How to Identify When Scanning Is Started and Finished
**Scanning has started**  
you can see list of source tables in a Structure mapping interface. 

**Scanning has finished**  
Log message with log.level < 10
```json
2024-11-04 11:15:31,947 [DEBUG] [main] response body: b'{\n    "type": "idle",\n    "readCount": 0,\n    "data": {\n        "idle": true\n    },\n    "status": "none"\n}'
```

## Check Results on the Platform
1. Open the converter section.
2. Find dataset you've added and open structure mapping.
3. Check values with source explorer section. 

![GIF](./media/check-scan-results.gif)
