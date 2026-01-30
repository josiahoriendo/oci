# Provision Necessary Resources

## Introduction

In this lab, you’ll prepare the foundation for the AI Meeting Summarizer workflow. You will create a dedicated compartment, three Object Storage buckets (uploads, transcripts, and results), and an Event rule that triggers a Function whenever a new object is uploaded. This enables an event-driven pipeline on OCI with clear separation of data and least-privilege access.

Estimated Time: 20 minutes

### Objectives

In this lab, you will:

* Create a compartment for the workshop resources.
* Create private Object Storage buckets for uploads, transcripts, and results.
* Enable events on the uploads bucket and create an Event rule for object creation.

### Prerequisites (Optional)

*List the prerequisites for this lab using the format below. Fill in whatever knowledge, accounts, etc. is needed to complete the lab. Do NOT list each previous lab as a prerequisite.*

This lab assumes you have:

* An Oracle Cloud account with permissions to create compartments, buckets, and Events.
* Familiarity with the OCI Console (helpful but not required).
* Region chosen for all resources (keep everything in the same region).

*This is the "fold" - below items are collapsed by default*

## Task 1: Create a compartment

1. In the OCI Console, open the navigation menu and go to Identity & Security → Compartments.

2. Click Create compartment.

3. Enter:
   - Name: ai-meeting-summarizer
   - Description: Workshop resources for AI Meeting Summarizer
   - Parent compartment: Your tenancy root or a suitable parent

4. Click Create compartment.

## Task 2: Concise Task Description

1. Step 1 - tables sample

  Use tables sparingly:

  | Column 1 | Column 2 | Column 3 |
  | --- | --- | --- |
  | 1 | Some text or a link | More text  |
  | 2 |Some text or a link | More text |
  | 3 | Some text or a link | More text |

2. You can also include bulleted lists - make sure to indent 4 spaces:

    - List item 1
    - List item 2

3. Code examples

    ```
    Adding code examples
  	Indentation is important for the code example to appear inside the step
    Multiple lines of code
  	<copy>Enclose the text you want to copy in <copy></copy>.</copy>
    ```

4. Code examples that include variables

	```
  <copy>ssh -i <ssh-key-file></copy>
  ```

## Learn More

*(optional - include links to docs, white papers, blogs, etc)*

* [URL text 1](http://docs.oracle.com)
* [URL text 2](http://docs.oracle.com)

## Acknowledgements
* **Author** - <Name, Title, Group>
* **Contributors** -  <Name, Group> -- optional
* **Last Updated By/Date** - <Name, Month Year>


# Lab 1: Set up compartments, Object Storage buckets, and an Event rule for object creation

## Introduction
In this lab, you’ll prepare the foundation for the AI Meeting Summarizer workflow. You will create a dedicated compartment, two Object Storage buckets (uploads and results), and an Event rule that triggers a Function whenever a new object is uploaded. This enables an event-driven pipeline on OCI with clear separation of data and least-privilege access.

Estimated Time: 20–30 minutes

### Objectives
- Create a compartment for the workshop resources.
- Create private Object Storage buckets for uploads and results.
- Enable events on the uploads bucket and create an Event rule for object creation.

### Prerequisites
This lab assumes you have:
- An Oracle Cloud account with permissions to create compartments, buckets, and Events.
- Familiarity with the OCI Console (helpful but not required).
- Region chosen for all resources (keep everything in the same region).

Tip: You can perform these steps in the OCI Console or using Cloud Shell/CLI. Instructions below use the Console.

## Task 1: Create a compartment
1. In the OCI Console, open the navigation menu and go to Identity & Security → Compartments.
2. Click Create compartment.
3. Enter:
   - Name: ai-meeting-summarizer
   - Description: Workshop resources for AI Meeting Summarizer
   - Parent compartment: Your tenancy root or a suitable parent
4. Click Create compartment.

## Task 2: Create Object Storage buckets
Create two private buckets in the same region and namespace.

A. Uploads bucket
1. Go to Observability & Management → Object Storage & Archive Storage → Buckets.
2. Select the ai-meeting-summarizer compartment.
3. Click Create bucket.
4. Enter:
   - Name: upload
   - Default storage tier: Standard
   - Encryption: Use Oracle-managed keys (or choose a customer-managed key if required)
   - Visibility: Private
5. Click Create.

B. Results bucket
1. Still under the same compartment, click Create bucket again.
2. Enter:
   - Name: transcript (or video-results if you prefer)
   - Default storage tier: Standard
   - Encryption: Oracle-managed keys (or customer-managed key as required)
   - Visibility: Private
3. Click Create.

Note: Record your Object Storage namespace (visible at the top of Buckets page). You’ll use it in later labs.

## Task 3: Enable events on the uploads bucket
1. Open the upload bucket.
2. In Bucket details, ensure Emit Object Events is enabled (toggle On). If disabled, click Edit and enable it.
3. Save changes.

## Task 4: Create an Event rule for object creation
1. Navigate to Developer Services → Events Service → Rules.
2. Make sure you’re in the ai-meeting-summarizer compartment.
3. Click Create rule and enter:
   - Name: on-object-create
   - Description: Trigger function when a new object is created in upload bucket
   - Rule condition: Use Event Type = Object - Create (com.oraclecloud.objectstorage.createobject)
   - Condition filter (Attributes):
     - bucketName equals upload
     - namespace equals your namespace (optional but recommended)
4. Actions:
   - Click Add action → Select Action Type: Functions
   - Choose the application and function (you can leave this blank if you will create the function in the next lab; you can return to add it later)
5. Click Create rule.

Note: If the function is not yet deployed, create the rule now without the action, then edit the rule later to add the Functions action once your Transcribe Function is available.

## Task 5: (Optional) Create prefixes in the results bucket
To keep outputs organized, you can pre-create logical folder prefixes:
- transcriptions/
- summaries/
- status/
This is optional (prefixes are created on first write), but useful for browsing.

## Validation
- In the Buckets list, confirm both upload and transcript buckets exist in the ai-meeting-summarizer compartment and are Private.
- In the upload bucket, confirm Emit Object Events = On.
- In Events → Rules, confirm on-object-create is Enabled and scoped to the correct compartment.

If everything looks good, proceed to the next lab to configure IAM policies and deploy the Transcribe Function.