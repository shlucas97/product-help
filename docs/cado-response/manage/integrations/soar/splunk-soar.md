---
title: Splunk SOAR
hide_title: true
sidebar_position: 5
---

# Splunk SOAR Integration

Splunk SOAR is a Security Orchestration, Automation, and Response (SOAR) system. The Splunk SOAR platform combines security infrastructure orchestration, playbook automation, and case management capabilities to integrate your team, processes, and tools together including integrating with Cado.

## Getting Started
 
Before getting started, please see the **[Integrations Overview >](../api-overview.md)** page for general instructions on setting up the Cado platform for integration with third-party tools.

Below are the instructions for setting up the Cado and Splunk SOAR integration:

1. **[Create an API Access Key](#create-an-api-access-key)**
2. **[Install and configure the Cado Response app in Splunk SOAR](#install-and-configure-the-cado-response-app)**
3. **[Set your configuration variables](#configuration-variables)**
4. **[Test your connection](#test-your-connection)**
5. **[Setup your Playbooks](#playbooks)**

### Create an API Access Key

In order to allow third-party services to authenticate and interact with a Cado instance, you will need to **[Create an API Access Key](../api-overview.md#creating-and-storing-an-api-access-key)** for use within Splunk SOAR.

:::tip
In addition to the API Access Key, please ensure that the Splunk SOAR platform has access (network, firewall, etc) to make API calls to the Cado platform.
:::

### Install and Configure the Cado Response App

In order to install and configure the Cado Response Splunk SOAR App, you will need the below information:

- The latest version of the Cado Response Splunk SOAR App Add-on from the [**Splunkbase Marketplace**](https://splunkbase.splunk.com/app/6181/)
- The URL to Cado, followed by `/api/v2`
- The API Access Key

To install the Cado Response App, from within Splunk SOAR, navigate to **Apps** and click the **INSTALL APP** button in the top right of the page, then upload the Cado Response tarball and click **INSTALL**.

If you have successfully installed the Cado Response App you will see it appear under your **Unconfigured Apps**. Next, do the following:
1. Click **CONFIGURE NEW ASSET** on the Cado Response App. 
2. Fill in the Asset Info fields with a name and description. 
3. Navigate to Asset Settings 
4. input your URL for the Cado platform
5. input your **Access token** you generated earlier. 
6. Click **SAVE** and **TEST CONNECTIVITY**. 

If all is successful, you should see a `Test Connectivity Passed` message.

#### Configuration Variables

The below configuration variables are required to operate Cado via the Splunk SOAR App. These are specified when configuring an asset in Splunk SOAR.

| VARIABLE | REQUIRED | TYPE | DESCRIPTION |
| -------- | -------- | ---- | ----------- |
| base_url | required | string | URL for the Cado Platform e.g. `https://cadoresponseurl/api/v2` |
| access_token | required | password | Access token |
| default_project | optional | numeric | A project ID in Cado that can be used if a project ID is not specified in an action. |
| default_bucket | optional | string | A default bucket in Cado |
| default_region | optional | string | AWS region, e.g. us-east-1, that can be used if a region is not specified in an action. |

When running Actions, if the above inputs are required, they would be entered automatically into the response fields, but you are still able to replace them if you wish to use different values.

### Test your connection

When Cado is configured, you can validate your connection to make sure everything is set up correctly.

Navigate to **Home -> Apps -> Cado Response Asset -> Asset Settings**. If you haven't already, Edit and enter the Cado URL and Secret Key. Save and Test Connectivity to ensure everything is connected as expected.

If the following was successful, then you have correctly setup Splunk SOAR to interact with the Cado Response API.

## Atomic Actions

Atomic Actions are the basic actions that can be chained together in _Playbooks_ that can be run to perform a specific task or workflow.

### List Projects

- Name: `list projects`
- Identifier: list_projects
- Type: Investigate
- Description: List projects that are currently active in the Cado Response platform.

**Action Parameters**

No Parameters required for this action.

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | ------------- |
| action_result.data.\*.\*.id | string | 1 |
| action_result.data.\*.\*.case_name | string | My First Project |
| action_result.status | string | success |

### List EC2 instances

- Name: `list instances`
- Identifier: list_instances
- Type: Investigate
- Description: List EC2 instances that are currently visible in the Cado Response platform.

**Action Parameters**

| PARAMETER | REQUIRED | DESCRIPTION | TYPE | EXAMPLE VALUE |
| --------- | -------- | ----------- | ---- | ------------- |
| project_id | required | Project identifier as listed in the output of "list projects" action | numeric | 1 |

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | ------------- |
| action_result.data.\*.\*.region | string | us-east-1 |
| action_result.data.\*.\*.id | string | i-1234567890abcdef0 |
| action_result.data.\*.\*.instance_name | string | CadoResponse-Test |
| action_result.data.\*.\*.instance_type | string | t3a.2xlarge |
| action_result.status | string | success |
| action_result.parameter.project_id | numeric | 1 |

### List S3 buckets

- Name: `list buckets`
- Identifier: list_buckets
- Type: Investigate
- Description: List S3 buckets that are currently visible to the Cado Response platform.

**Action Parameters**

| PARAMETER | REQUIRED | DESCRIPTION | TYPE | EXAMPLE VALUE |
| --------- | -------- | ----------- | ---- | ------------- |
| project_id | required | Project identifier as listed in the output of "list projects" action | numeric | 1 |

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | ------------- |
| action_result.data.*.*.bucket_name | string | test-bucket |
| action_result.parameter.project_id | numeric | 1 |
| action_result.status | string | success |

### List Pipelines

- Name: `list pipelines`
- Identifier: list_pipelines
- Type: Investigate
- Description: List pipelines for specified project in Cado Response platform.

**Action Parameters**

| PARAMETER | REQUIRED | DESCRIPTION | TYPE | EXAMPLE VALUE |
| --------- | -------- | ----------- | ---- | ------------- |
| pipeline_id | required | Pipeline identifier as listed in the output of "list pipelines" action | numeric | 6 |

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | -------------- |
| action_result.data.\*.pipelines.\*.pipeline_id | numeric | 6 |
| action_result.data.\*.pipelines.\*.name | string | Acquiring i-1234567890abcdef0 |
| action_result.data.\*.pipelines.\*.pipeline_type | string | Acquisition |
| action_result.data.\*.pipelines.\*.evidence_id | numeric | 4 |
| action_result.data.\*.pipelines.\*.summary.total | numeric | 18 |
| action_result.data.\*.pipelines.\*.summary.failure | numeric  | 0 |
| action_result.data.\*.pipelines.\*.summary.pending | numeric | 0 |
| action_result.data.\*.pipelines.\*.summary.running | numeric | 0 |
| action_result.data.\*.pipelines.\*.summary.success | numeric | 18 |
| action_result.data.\*.pipelines.\*.summary.cancelled | numeric | 0 |
| action_result.data.\*.pipelines.\*.terminated | string | true |
| action_result.parameter.project_id | numeric | 1 |
| action_result.status | string | success |

### Get a pipeline

- Name: `get pipeline`
- Identifier: get_pipeline
- Type: Investigate
- Description: Get pipeline details for specified pipeline ID in Cado Response platform.

**Action Parameters**

| PARAMETER | REQUIRED | DESCRIPTION | TYPE | EXAMPLE VALUE |
| --------- | -------- | ----------- | ---- | ------------- |
| pipeline_id | required | Pipeline identifier as listed in the output of "get pipelines" action | numeric | 6 |

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | ------------- |
| action_result.data.\*.pipeline_id | numeric | 6 |
| action_result.data.\*.subtasks.\*.name | string | Acquisition: Create a processing pipeline for each copied EC2 volume |
| action_result.data.\*.subtasks.\*.start_time | numeric | 1633025565.147805 |
| action_result.data.\*.subtasks.\*.finish_time | numeric | 1633031638.334756 |
| action_result.data.\*.subtasks.\*.execution_duration | numeric | 6073 |
| action_result.data.\*.subtasks.\*.state | string | SUCCESS |
| action_result.parameter.pipeline_id | numeric | 6 |
| action_result.status | string | success |

### Loop Until Terminated

- Name: `loop pipeline`
- Identifier: loop_pipeline
- Type: Investigate
- Description: Get pipeline details every 60 seconds for specified pipeline ID in Cado Response platform until it is terminated.

**Action Parameters**

| PARAMETER | REQUIRED | DESCRIPTION | TYPE | EXAMPLE VALUE |
| --------- | -------- | ----------- | ---- | ------------- |
| pipeline_id | required | Pipeline identifier as listed in the output of "get pipelines" action | numeric | 6 |

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | -------------- |
| action_result.data.\*.pipeline_id | numeric | 6 |
| action_result.data.\*.subtasks.\*.name | string | Acquisition: Create a processing pipeline for each copied EC2 volume |
| action_result.data.\*.subtasks.\*.start_time | numeric | 1633025565.147805 |
| action_result.data.\*.subtasks.\*.finish_time | numeric | 1633031638.334756 |
| action_result.data.\*.subtasks.\*.execution_duration | numeric | 6073 |
| action_result.data.\*.subtasks.\*.state | string | SUCCESS |
| action_result.parameter.pipeline_id | numeric | 6 |
| action_result.status | string | success |

### Create Project

- Name: `create project`
- Identifier: create_project
- Type: n/a
- Description: Create a project in Cado Response platform.

**Action Parameters**

| PARAMETER | REQUIRED | DESCRIPTION | TYPE | EXAMPLE VALUE |
| --------- | -------- | ----------- | ---- | ------------- |
| project_name | required | User provided Project name | string | My First Project |

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | ------------- |
| action_result.status | string | success |
| action_result.data.*.id | numeric | 1 |
| action_result.parameter.project_name | string | My First Project |
| action_result.data.*.msg | string | Created |

### Capture EC2 instance

- Name: `capture instance`
- Identifier: capture_instance
- Type: n/a
- Description: Capture a specific EC2 instance for processing and analysis on the Cado Response platform.

**Action Parameters**

| PARAMETER | REQUIRED | DESCRIPTION | TYPE | EXAMPLE VALUE |
| --------- | -------- | ----------- | ---- | ------------- |
| bucket | required | S3 bucket name as listed in the output of "list bucket" action | string | test-bucket |
| region | required | AWS instance region as listed in the output of "list instances" action | string | us-east-1 |
| project_id | required | Project identifier as listed in the output of "list projects" action |numeric | 1 |
| instance_id | required | AWS instance ID as listed in the output of "list instances" action | string | i-1234567890abcdef0 |

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | ------------- |
| action_result.data.\*.pipelines.pipeline_id | numeric | 1 |
| action_result.data.\*.name | string | Acquiring i-1234567890abcdef0 |
| action_result.parameter.project_id | numeric | 1 |
| action_result.parameter.region | string | us-east-1 |
| action_result.parameter.instance_id | string | i-1234567890abcdef0 |
| action_result.parameter.bucket | string | test-bucket |
| action_result.status | string | success |

### Capture S3 bucket

- Name: `capture bucket`
- Identifier: capture_bucket
- Type: n/a
- Description: Capture contents of a specific S3 bucket processing and analysis on the Cado Response platform.

**Action Parameters**

| PARAMETER | REQUIRED | DESCRIPTION | TYPE | EXAMPLE VALUE |
| --------- | -------- | ----------- | ---- | ------------- |
| bucket | required | S3 bucket name as listed in the output of "list bucket" action | string | example-bucket |
| project_id | required | Project identifier as listed in the output of "list projects" action |numeric | 1 |

**Action Output**

| DATA PATH | TYPE | EXAMPLE VALUE |
| --------- | ---- | ------------- |
| action_result.data.\*.\*.pipelines.pipeline_id | string | 6 |
| action_result.data.\*.\*.pipelines.project_id | string | 1 |
| action_result.data.\*.\*.pipelines.evidence_id | string | 4 |
| action_result.data.\*.\*.pipelines.pipeline_type | string | acquisition |
| action_result.status | string | success |
| action_result.parameter.bucket | string | test-bucket |
| action_result.parameter.project_id | numeric | 1 |

## Playbooks

_If you have a good grasp of the basics of Splunk SOAR, feel free to skip this section._

To create a new playbook, navigate to Playbooks from the Splunk SOAR dashboard. Click the **+ PLAYBOOK** button in the top right.

Using the graphic playbook editor, you can drag and drop the required elements into place from the blue nodes, and configure them one by one as you place them, until you have a playbook ready.

![Splunk SOAR Playbook](/img/phantom-playbook.png)

Playbooks are made up of multiple blocks that carry out a workflow. There are 3 main types of blocks:

- Execute actions (these include the atomic actions detailed above)
- Process filters
- Human input

The example below makes use of both execute actions and human input. Prompts pop up and asks the user for an input. This input can then be passed to the actions as action parameters and act accordingly.

### Capture EC2 Instance

This section covers a quick tutorial in creating a basic workflow (or playbook in Splunk SOAR terms) that:

- Creates a new Project
- Triggers an EC2 Disk Acquisition
- Loops over a pipeline until it has been terminated

#### Prompt: Set Project Name

From the START block you can drag the blue node to add a new block. Select the Prompt option. Under the **Advance Settings** -> **General Settings** you can set a custom name, i.e. `Set Project Name`. You can include a Message, so the user knows what to include.

#### Action: Creating the Project

Drag the blue node from the prompt you just added to add an action. Select your configured Cado Response App to view the available actions you can carry out. You will want the **create project** action. Again here you can change the action name under **Advanced Settings**.

You can configure the action by clicking the Cado Response Asset and entering the output from the previous prompt into the **project_name** field. Do this by clicking on the field where a sidebar will pop up with a list of blocks currently used in the playbook. Select the prompt you just created, e.g. `Set_Project_Name`. This will open another sidebar with the data path outputs generated from the prompt. Select `summary.responses.0`. SAVE and the action will be created.

#### Action: List S3 Buckets

From the last action add the **list bucket** action. Configure the project_id to be the ID output from the **create project** action, i.e. `create_project_1:action_result.data.*.id`

#### Prompt: Set S3 Bucket

Add a new prompt block from the previous action called `Set S3`. Click the **ADD MESSAGE PARAMETERS** button. This will add **{0}** to the message which acts as a placeholder for a message parameter. In the message parameter field select the list bucket action you just created followed by the bucket_name action results i.e. `list_bucket_1:action_result.data.*.*.bucket_name`.

#### Action: List EC2 Instances

Add the list instances action next with the project_id field containing the ID output from the **create project** action, i.e. `create_project_1:action_result.data.*.id`.

#### Prompt: Set EC2 Instance

Similar to the **Set S3** prompt, add a new prompt block with 2 message parameters. The first being the list instances region (`list_instances_1:action_result.data.*.*.region`) and the second being the list instances instance ID (`list_instances_1:action_result.data.*.*.id`).

In the responses part of the configuration for the action add the first to be `Region` and the second to be `Instance ID`.

#### Action: Capture EC2 Instance

Next add the **capture instance** action, this will need 4 parameters configured. **project_id** will take the ID result from the create project action. Instance ID and region will take their respective responses from the previous prompt and bucket will take the response from the set S3 prompt.

- project_id: `create_project_1:action_result.data.*.id`
- instance_id: `Set_EC2:action_result.summary.responses.1`
- region: `Set_EC2:action_result.summary.responses.0`
- bucket: `Set_S3:action_result.summary.responses.0`

#### Action: Loop Pipeline until Terminated

Add the final action which will be the **loop pipeline** action. Fill in the **pipeline_id** field with the name result from the capture instance action: `capture_instance_1:action_result.data.*.name`.

Connect this final action block to the **END** block and the playbook is complete. Be sure to **SAVE** using the top right button, and then you can run your playbook.
