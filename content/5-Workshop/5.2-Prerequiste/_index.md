---

title: "Preparation Steps"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
----

#### IAM Permissions

## Required Permissions

* AdministratorAccess
* AmazonBedrockFullAccess
* AWSCodeBuildAdminAccess
* AWSCodeBuildDeveloperAccess
* BedrockAgentCoreFullAccess

## Create a User and Assign Permissions

1. Go to **IAM** → **Users** → select **Create user**.
2. Add the permissions listed above.
3. Complete the user creation and save the Access Key if you need it for the SDK.

![iam](/aws/static/images/5-Workshop/5.2-Prerequisite/iam.png)

#### Download AWS CLI

Download AWS CLI:
[**AWS CLI Link**](https://s3.amazonaws.com/aws-cli/AWSCLI64PY3.msi)

Then install it following the instructions.

![CLI](/aws/static/images/5-Workshop/5.2-Prerequisite/cli.png)

## UV Management Setup

#### 1. Why use UV?

UV is fast, lightweight, and manages environments better than pip.

#### 2. Install UV on Windows

Run:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Add UV to PATH:

```powershell
$env:Path = "C:\Users\leamo\.local\bin;$env:Path"
```

![uvpath](/aws/static/images/5-Workshop/5.2-Prerequisite/uv.png)

> Restart your machine to apply the new PATH.

#### 3. Initialize a UV Environment

Inside your project directory:

```bash
uv init
```

![runuv](/aws/static/images/5-Workshop/5.2-Prerequisite/uvinit.png)

Then select the environment in VS Code.

## Connect Your Machine to AWS CLI

Go back to IAM to create an Access Key.

#### Create Access Key and Configure AWS CLI

1. In the user page: **Security credentials** → **Create access key**
2. Choose **Command Line Interface (CLI)**

#### Configure AWS CLI

Run:

```bash
aws configure
```

Fill in:

* **AWS Access Key ID**
* **AWS Secret Access Key**
* **Default region name** (example: `ap-southeast-1`)
* **Default output format**

```bash
json
```

![conectCLI](/aws/static/images/5-Workshop/5.2-Prerequisite/conectcli.png)

## Start AWS CLI AgentCore

Run:

```bash
uv run which agentcore
```

After running, it will download all necessary libraries for AWS AgentCore.

![run agentcore](/aws/static/images/5-Workshop/5.2-Prerequisite/whichagent.png)

## Create Groq API

Go to [Groq](https://console.groq.com/keys) and create an API key as shown.
These external tools support RAG and are integrated through AWS AgentCore.

![groqapi](/aws/static/images/5-Workshop/5.2-Prerequisite/groqapi.png)
