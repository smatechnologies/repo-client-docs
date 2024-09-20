---
slug: '/'
sidebar_label: 'Installation'
---

# Installation
The installation of the Repo Client consists of extracting the information from the zip file into a folder on the system, configuring the Connector.config file and creating the required repositories in the selected environment. The extracted data contains all the required files and the Java version (for Windows) required to execute the Repo Client CLI.

If the target installation is to be a Linux system, the appropriate Linux Java 11 Open JDK must be downloaded and installed in the /java folder.
The following repoclient.sh can be created 

```
#!/bin/bash
SCRIPT_DIR=$(dirname "$0")
cd $SCRIPT_DIR
java -cp repo-client.jar com.smatechnologies.repo.client.RepoCLI "$@"´

```
## Encryption

Certain information needs to be stored encrypted in files The **Encrypt.exe** utility must be used to encrypt the information and the encrypted value
should be copied into the file.

```
Encrypt.exe -v "abc"

[10:31:04] [INFO ] --------------------------------------------------------------------- 
[10:31:04] [INFO ] EncryptValue          : Version 23.0.0 
[10:31:04] [INFO ] --------------------------------------------------------------------- 
[10:31:04] [INFO ] -v  (value)           : abc 
[10:31:04] [INFO ] --------------------------------------------------------------------- 
[10:31:04] [INFO ] ev : 59574a6a 
[10:31:04] [INFO ] --------------------------------------------------------------------- 
```

## Configuration
Edit the Connector.config file inserting the information concerning the repository in the [REPO] section.

```
[GENERAL]
DEBUG=OFF

[REPO]
TYPE=Git
ORG=<organization name>
PROJECT=<project>
PAT=<encrypted personal access token>
SERVER-REPO=OPCON_SERVERS
WORKFLOW-REPO=OPCON_WORKFLOWS
TRANSFORMATION-REPO=OPCON_TRANSFORMATIONS
```

Property Name            | Value
------------------------ | -----------
**TYPE**                 | Indicates the repository type (value Git indicates Azure DevOps)
**ORG**                  | The name of the organization associated with Azure DevOps environment
**PROJECT**              | The name of the project within the organization
**PAT**                  | A personal access token that can be used to access the repositories (must be encrypted using the Encrypt.exe program)
**SERVER-REPO**          | The name of the repository that contains the OpCon system definitions for extracting and inserting workflow definitions
**WORKFLOW-REPO**        | The name of the repository that will contain the workflow definitions
**TRANSFORMATION-REPO**  | The name of the repository that contains the transformation rules

## Repositories
Create the OPCON_SERVERS, OPCON_WORKFLOWS and OPCON_TRANSFORMATIONS repositories in the Azure DevOps environment.

Once the repositories are created create the required branches.

OPCON_SERVERS create branch **servers** from **main**
OPCON_WORKFLOWS create the required application group branch from **main**.
OPCON_TRANSFORMATIONS create the **servers** and **Applications** branches from **main**.



