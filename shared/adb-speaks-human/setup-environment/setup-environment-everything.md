# Set up the Workshop Environment

<!---
comments syntax
--->

## Introduction

This workshop focuses on teaching you how to setup and use generative AI to query your data using natural language from a SQL prompt and from an application. To fast track using Select AI, you will deploy a ready-to-go environment using a terraform script that will:

* Provision your Autonomous Database instance with the required users and data
* Install the Select AI demo application that was built using APEX

The automation uses a predefined OCI Cloud Stack Template that contains all the resources that you need. You'll use OCI Resource Manager to deploy this template and make your environment available in just a few minutes. You can use Resource Manager for your own projects. For more details, see the [Overview of Resource Manager](https://docs.oracle.com/en-us/iaas/Content/ResourceManager/Concepts/resourcemanager.htm) Oracle Cloud Infrastructure documentation.

Estimated Time: 5 minutes.

### Objectives

In this lab, you will:

* Run the stack to perform all the prerequisites required to analyze data

## Task 1: (Optional) Create an OCI Compartment

A compartment is a collection of cloud assets, such as compute instances, load balancers, databases, and so on. By default, a root compartment was created for you when you created your tenancy (for example, when you registered for the trial account). It is possible to create everything in the root compartment, but Oracle recommends that you create sub-compartments to help manage your resources more efficiently.

If you are using an Oracle LiveLabs-provided sandbox, you don't have privileges to create a compartment and should skip this first task. Oracle LiveLabs has already created a compartment for you and you should use that one. Even though you can't create a compartment, you can review the steps below to see how it is done.

1. Log in to the **Oracle Cloud Console**. On the **Sign In** page, select your tenancy, enter your username and password, and then click **Sign In**. The **Oracle Cloud Console** Home page is displayed.

2. Open the **Navigation** menu.

    ![Click the Navigation menu.](./images/click-navigation-menu.png =65%x*)

3. Click **Identity & Security**. Under **Identity**, click **Compartments**.

       ![The navigation path to Compartments is displayed.](./images/navigate-compartment.png =80%x*)

4. On the **Compartments** page, click **Create Compartment**.

   ![The Compartments page is displayed. The Create Compartment button is highlighted.](./images/click-create-compartment.png =70%x*)

5. In the **Create Compartment** dialog box, enter an appropriate name such as **`training-adw-compartment`** in the **Name** field and a description such as **`Training ADW Compartment`** in the **Description** field.

6. In the **Parent Compartment** drop-down list, select your parent compartment, and then click **Create Compartment**.

   ![On the completed Create Compartment dialog box, click Create Compartment.](./images/create-compartment.png =70%x*)

   The **Compartments** page is re-displayed and the newly created compartment is displayed in the list of available compartments. You can use the compartment for your cloud services!

   ![The newly created compartment is highlighted with its status as Active.](./images/compartment-created.png =70%x*)

## Task 2: Provision an ADB Instance, Load Data, and Install the Select AI Demo Application

Use an OCI Cloud Stack to set up your workshop environment by creating an ADB instance, upload the data to the instance, and install the Select AI demo application that was built using APEX.

1. Deploy the required cloud resources for this workshop using the OCI Resource Manager. Click the button below:

    <a href="https://cloud.oracle.com/resourcemanager/stacks/create?region=home&zipUrl=https://github.com/oracle-devrel/terraform-oci-oracle-cloud-foundation/releases/download/v1.0.0/Deploy-ChatDB-Autonomous-Database-Select-AI-demonstration-RM.zip" class="tryit-button">Deploy workshop</a>

    The automation uses a predefined OCI Cloud Stack Template that contains all of the resources that you will need in this workshop. You'll use OCI Resource Manager to deploy this template and make your environment available in just a few minutes. Your first step will be to log in to Oracle Cloud. Next, you will land on the Resource Manager page where you will kick off a job that will do the following:
    * Create a new Autonomous Database named **`MovieStreamWorkshop`** by default; however, you can replace the database name with your own name.
    * Create a new user named **`moviestream`**
    * Create movie related tables and views in the **`moviestream`** schema
    * Grant the required privileges to perform various actions in the workshop
    * Download the **Autonomous Database Select AI** APEX application

    >**Note:** For detailed information about Resource Manager and managing stacks in Resource Manager, see the [Overview of Resource Manager](https://docs.oracle.com/en-us/iaas/Content/ResourceManager/Concepts/resourcemanager.htm#concepts__package) and [Managing Stacks](https://docs.oracle.com/en-us/iaas/Content/ResourceManager/Tasks/stacks.htm) documentation.

2. After you log in to your Oracle Cloud account, the **Create stack** page is displayed. In the **Stack information** step 1 of the wizard, select the **I have reviewed and accept the Oracle Terms of Use** check box. In the **Create in compartment** drop-down list, select your desired compartment. Accept the default values for the rest of the fields, and then click **Next**.

    >**Note:** When you access the **Create Stack** page, the **US East (Ashburn)** region is selected by default. This is where the stack will be created. If you want to create the stack in a different region, select that region from the **Regions** drop-down list in the Console's banner.

  ![The Stack information step 1 of the wizard](./images/create-stack.png "")

3. In the **Configure variables** step 2 of the wizard, provide the following:
    * **Region:** Select the target region for the new Autonomous Database instance. In our example, we chose the `ca-toronto-1` region.
    * **Compartment:** Select the target compartment for the new Autonomous Database instance.
    * **Database Name:** The default database name is **`MovieStreamWorkshop`**. You can replace this name with your own name but that is optional. In our example, we changed the database name to **``TrainingAIWorkshop``**. The database name must contain only letters and numbers, starting with a letter, and between 12 and 30 characters long. The name cannot contain the double quote (") character, space, underscore "_", or the username `admin`.

    >**Important:** Your database name that you choose must be unique in the tenancy that you are using; otherwise, you will get an error message.
    
    * **Do you want an always Free Oracle Autonomous Database instance?** Accept the default **`false`** value. Select **`true`** from the drop-down list if you want to deploy an Always Free database.

        ![Provision an always free ADB instance](./images/provision-always-free.png "")

    * **Password:** Enter a password for the `ADMIN` user of your choice such as **`Training4ADW`**. **Important**: Make a note of this password as you will need it to perform later tasks.
    * **Secret API key used to connect to AI model:** Access to OpenAI is being provided for the duration of this event. Click the button below and then copy and paste the secret API key:
    <a href="https://adwc4pm.objectstorage.us-ashburn-1.oci.customer-oci.com/n/adwc4pm/b/select-ai-workshop/o/select-ai-chat-workshop.html" class="tryit-button">Obtain secret key</a>


    * For the other fields, accept the default selections.
    
    ![The Configure variables step 2 of the wizard](./images/configure-variables.png =110%x*)

4. Click **Next**. If clicking **Next** does not take you to the page 3 of the wizard, check the **Region** field. It may have been reset.

    ![Click next in step 2 of the wizard](./images/click-next.png "")

5. In the **Review** step 3 of the wizard, review your configuration variables and make any necessary changes on the previous pages. If everything looks good, then it's time for you to create and apply your stack! Ensure that the **Run apply** check box is checked, and then click **Create**.

    ![Click Create](./images/click-create.png "")

6. The **Job details** page is displayed. The initial status (in orange color) is **ACCEPTED** and then **IN PROGRESS**.

  ![Job in progress](./images/in-progress.png "")

  If the job completes successfully, the status changes to **SUCCEEDED** (in green color). This process can take 5 to 10 minutes to complete.

  ![Job has been successful](./images/stack-success.png "")

7. Scroll-down to the **Resources** section at the bottom of **Job details** page, and then click **Outputs**. The keys and values are displayed in the **Outputs** section.

    ![User details](./images/output.png "")

8. Save the values for the following keys in a text editor of your choice as you will need this information later. For the **`select_ai_demo_url`** value, click the **Copy** button in that row to copy the value into the clipboard, and then paste it into your text editor. _This is the URL that you will use later to launch the **Autonomous Database Select AI** demo application._

    * **`adb_user_name`**
    * **`adb_user_password`**
    * **`select_ai_demo_url`**

      ![Save values in file.](./images/save-values.png "")

## Task 3: Review Your Deployment

1. Let's view the newly created stack and job. From the Console, open the **Navigation** menu.

    ![Click the Navigation menu.](./images/click-navigation-menu.png =60%x*)

2. Click **Developer Services**. Under **Resource Manager**, click **Stacks**.

    ![Navigate to stacks](./images/navigate-stacks.png "")

    The newly created stack is displayed in the **Stacks** page. Select the region and compartment that you specified when you deployed the stack.
    
    ![The stack is displayed](./images/stacks-page.png "")
    
3.  Click the stack name. The **Stack details** page is displayed.

    ![Click Jobs](./images/stack-details-page.png "")

4.  In the **Jobs** section, click the job name. The **Job details** page is displayed.

    ![Job details page](./images/job-details.png "")

    You can use the **Logs** section to view the created resources such as the **moviestream** user. You can also use the **Output** link in the **Resources** section to find out the values of different keys.

    The **Logs** section is useful when you have a failed job and you try to find out why it failed. In the following failed job example, we scrolled down the log and then searched for text in red font color which describes the potential problem. In this specific example, we specified a database name with an underscore which does not meet the requirements for a database name.

    ![Failed job](./images/failed-job.png "")

## Task 4: Navigate to Your New Autonomous Database Instance

Let's view the newly provisioned ADB instance.

1. From the Console, open the **Navigation** menu and click **Oracle Database**. Under **Autonomous Database**, click **Autonomous Data Warehouse**.

2. On the **Autonomous Databases** page, select the _compartment and region_ that you specified in the **Configure variables** step 2 of the wizard. The Autonomous Database that was provisioned by the stack is displayed, **``TrainingAIWorkshop``**.

    ![The Autonomous Databases page](./images/adb-instances.png "")

You may now proceed to the next lab.

## Learn More

* [Using Oracle Autonomous Database Serverless](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/index.html)
* [Oracle Cloud Infrastructure](https://docs.cloud.oracle.com/en-us/iaas/Content/GSG/Concepts/baremetalintro.htm)
* [OpenAI API Get Started](https://platform.openai.com/docs/introduction)

## Acknowledgements

* **Author:** Lauran K. Serhal, Consulting User Assistance Developer
* **Contributor:** Marty Gubar, Product Manager
* **Last Updated By/Date:** Lauran K. Serhal, April 2024

Data about movies in this workshop were sourced from **Wikipedia**.

Copyright (c) 2024 Oracle Corporation.

Permission is granted to copy, distribute and/or modify this document
under the terms of the GNU Free Documentation License, Version 1.3
or any later version published by the Free Software Foundation;
with no Invariant Sections, no Front-Cover Texts, and no Back-Cover Texts.
A copy of the license is included in the section entitled [GNU Free Documentation License](files/gnu-free-documentation-license.txt)