--
slug: '/'
sidebar_label: 'Repo Client'
---

# Repo Client

The RepoCli.exe provides 3 functions:

- deploy			to push a workflow definitions from an Azure DevOps branch into an OpCon System.
- extract 		    to extract workflows from an OpCon System and place them directly in an Azure DevOps branch.
- simulate		    to simulate a workflow definitions deployment from an Azure DevOps branch.

## deploy

When performing a deploy of workflows to an OpCon system, it is possible to use wild cards (*/?) to select workflows to deploy within the defined branch or use an asterix (*) to deploy all workflow definition in the branch. 

If the target OpCon system definition includes a systemPrefix, the definitions are transformed appending the systemPrefix to major components like workflow name, global properties, resourcses, thresholds, scripts, agent names to create a unique definition.

It is also possible to transform individual items by defining transformation rules for the application group. 

It is possible to include a reference tag that is inserted into the schedule documentation field.  

Arguments used for deploy function.

Argument | Description
---------| ---------------------
**-t**   | is the task (value is deploy).
**-o**   | is the OpCon Server and must match a definition in the servers branch of the OPCON_SERVERS repository. It is not necessary to add the .json extension.
**-w**   | is the name of the workflow(s) to deploy - supports wildcards (*/?) or (*) to deploy all workflows in the application group. It is not necessary to add the .json extension.
**-wb**  | is the name of the workflow branch where the extracted workflow must be saved. This represents a group of workflows typically associated with the same appliction.
**-sb**  | is the name of the branch containing the server definition (default is servers, so if this is used, this argument is not necessary).
**-d**   | is a description that will be included in the schedule description field.

Examples

```
RepoCli.exe -t "deploy" -wb "Workflows-App1" -w "WINTEST" -o "QA_DESKTOP-QMQS7D3" -d "REL 1.0.1"  -sb "servers"
RepoCli.exe -t "deploy" -wb "Workflows-App1" -w "???TEST" -o "QA_DESKTOP-QMQS7D3" -d "REL 1.0.1"
RepoCli.exe -t "deploy" -wb "Workflows-App1" -w "WIN*" -o "QA_DESKTOP-QMQS7D3" -d "REL 1.0.1"
RepoCli.exe -t "deploy" -wb "Workflows-App1" -w "*" -o "QA_DESKTOP-QMQS7D3" -d "REL 1.0.1"

```

## extract
When performing an extract of workflows from an OpCon system, the software extracts all sub-schedules encountered in the requested extract and creates a 'package' consisting
of the schedule and sub-schedules.

It is also possible to extract multiple schedules by using wildcards (*/?) in the workflow name.
During the extract process, a new branch is created from the original branch using a date/timestamp.
All schedules extracted during the request will be placed in the created branch.
Following the completion of the request, a pull-request should be created to merge the created branch with the original branch. 

Arguments used for extract function.

Argument | Description
---------| ---------------------
**-t**   | is the task value is extract
**-o**   | is the OpCon Server and must match a definition in the servers branch of the OPCON_SERVERS repository. It is not necessary to add the .json extension.
**-w**   | is the name of the workflow to extract - supports wildcards (*/?).
**-wb**  | is the name of the workflow branch where the extracted workflow must be saved. This represents a group of workflows typically associated with the same appliction.
**-sb**  | is the name of the branch containing the server definition (default is servers, so if this is used, this argument is not necessary).

Examples

```
RepoCli.exe -t "extract" -wb "Workflows-App1" -w "WINTEST" -o "DESKTOP-QMQS7D3" -sb "servers"
RepoCli.exe -t "extract" -wb "Workflows-App3" -w "WIN*" -o "DESKTOP-QMQS7D3"

```
## simulate
When performing a simulate of workflows to an OpCon system, a check is made to determine if the deployment will succeed. 

Performs any transformations before performing the checks.

The function checks the following:
- if the schedule is compatible with the target OpCon environment.
- if any associated sub-schedules are either part of the definition or already defined on the target OpCon system.
- if roles contained in the definition are defined in the target OpCon system.
- if machines (agents) contained in the definition are defined in the target OpCon system.
- if machine groups contained in the definition are defined in the target OpCon system.
- if batch users contained in the definition are defined in the target OpCon system.
- if machine (agent) specific features are supported by the target machine (agent) on the target OpCon system.
- if any associated inter schedule dependencies are either part of the definition or already defined on the target OpCon system.

Arguments used for simulate function.

Argument | Description
---------| ---------------------
**-t**   | is the task value is simulate
**-o**   | is the OpCon Server and must match a definition in the servers branch of the OPCON_SERVERS repository. It is not necessary to add the .json extension.
**-w**   | is the name of the workflow to simulate. It is not necessary to add the .json extension.
**-wb**  | is the name of the workflow branch where the extracted workflow must be saved. This represents a group of workflows typically associated with the same appliction.
**-sb**  | is the name of the branch containing the server definition (default is servers, so if this is used, this argument is not necessary).

Examples

```
RepoCli.exe -t "simulate" -wb "Workflows-App1" -w "WINTEST" -o "QA_DESKTOP-QMQS7D3" -sb "servers"

```

```
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) Deployment Simulation of Schedule (DEVOPSEM.json) to OpCon System (DESKTOP-QMQS7D3) 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) Compatibility Check 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346)      
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) Definition compatible with target OpCon System 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) Sub-Schedule Check 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346)      
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) No missing Sub-Schedule definitions discovered 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.870] [INFO ] (RepoCLIImpl                 346) Role Check 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346)      
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) No missing Role definitions discovered 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) Machine Check 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346)      
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) No missing Machine definitions discovered 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) Machine Group Check 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346)      
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) No missing Machine Group definitions discovered 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) Batch User Check 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346)      
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) No missing Batch User definitions discovered 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) Job Machine Feature Requirements Check 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346)      
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) No missing Job Machine Feature Requirements discovered 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) Dependent Job Check 
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346)      
[24-06-20 06:46:19.871] [INFO ] (RepoCLIImpl                 346) No missing dependent Job definitions discovered 
[24-06-20 06:46:19.872] [INFO ] (RepoCLIImpl                 346) ------------------------------------------------------------------------------------- 

```