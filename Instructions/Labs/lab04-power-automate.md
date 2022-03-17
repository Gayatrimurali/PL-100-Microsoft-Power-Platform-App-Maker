# Lab 04: Power Automate

In this lab, you will create Power Automate cloud flows to automate various parts of the Company 311 solution.

The following have been identified as requirements you must implement to complete the project: 

  - Escalation, approval, and execution process for urgent maintenance issues 

  - Notify reporting user about the issue status changes 

  - How to use a business rule to implement logic.

## What you will learn

  - How to design data columns (in the data model) to support automation 

  - How to build a flow using Microsoft Dataverse Connector

  - How to use approvals 

## High-level lab steps

  - Add columns to support escalation 
  - Build flow to approve escalation  
  - Build flow to notify user of status change
  - Build approval as an adaptive card in Microsoft Teams 

## Prerequisites

* Must have completed **Lab 02.1: Data model and model-driven app**
* Must have completed **Lab 02.2: Business Process Flows and Business Rules**

## Things to consider before you begin

  - What is the most efficient way to identify urgent maintenance issues and escalate them

## Detailed steps  

### Exercise 1: Build notify flow

In this exercise, you create a flow that will notify the creator of a problem when the status changes.

#### Task 1: Create flow

In this task, you will create a flow that send notification when the status of problem report row changes.

1.  Navigate to the [Power Apps maker portal](https://make.powerapps.com/) and make sure you are in the correct environment.

1.  Select **Solutions** and click to open the **Company 311** solution.

1.  Click **+ New** and select **Automate**, click **Cloud Flow** and then click **Automate**.

1.  Select **Microsoft Dataverse** connector. If **Microsoft Dataverse** connector is not visible, select **Connectors** tab then select **Microsoft Dataverse**.

1.  Select **When a row is added, modified or deleted**.

    ![A screenshot of a border around the when a row is added, modified, or deleted option](04/media/image2.png)

1.  Select **Modified** for **Change type**, select **Problem Reports** for **Table name**, **Organization** for **Scope** and click **Show advanced options**.

1.  Enter **statuscode** for **Select columns** and **… Menu** button of the trigger step.

    ![A Screenshot with an arrow pointing to the ellipses icon for more options and a border around the select columns statuscode](04/media/image3.png)

1.  Select **Rename**.

1.  Rename the trigger step **When problem report status changes**.

1.  Click **+ New step**.

    ![A Screenshot with an arrow pointing to the add new step button](04/media/image4.png)

1. Select **Connectors** tab and then select **Microsoft Dataverse**. Select **Get a Row by ID**.

1. Select **Users** for **Table name**.

1. Click on the **Row ID** field, go to the Dynamic pane, search for **created** and click once on **Created By (Value)** to add it.

1. Click **Show advanced options** of the new step.

1. Enter **internalemailaddress** for **Select columns**.

1. Click on the **… Menu** button of the new step and select **Rename**.

1. Rename the step **Get problem creator**.

1. Click **+ New step**.

1. Search for **OUTLOOK**, select **OUTLOOK.COM** connector and select **Send an email (V2).**

1. Click to select the **To** Column and click **Switch to advanced mode**. Click on this button toggles show/hide of the dynamic pane.

    **NOTE**: If To field, asked to Sign in then sign in with your personal microsoft account.

    ![A Screenshot with an arrow pointing to the switch to advanced mode icon](04/media/image5.png)

1. Select the **Primary Email** Column from the **Get problem creator** step.

1. Enter **Problem report status change notification** for **Subject**.

1. Click to select the **Body** Column.

1. Type **The status of the problem you reported has changed.** and press the **[ENTER]** key.

1. Type **Problem Title:** go to the Dynamic pane, search for **title** and select **Title**.

1. Press the **[ENTER]** key.

1. Type **Current Status:** go to the Dynamic pane, select the **Expression** tab, paste the expression below, and click **OK**. This expression will show the label of the choice instead of the value.

    `triggerOutputs()?['body/_statuscode_label']`

1. Click on the **… Menu** button of the new step and select **Rename**.

1. Rename the **Notify problem creator**.

1. The step should now look like the image below.

    ![A screenshot of the modify problem creator window being to primary email, the subject being problem report status change notification, and the body being "The status of the problem you reported has changed" with the problem title and current status as trigger outputs below that](04/media/image7.png)

1. Scroll up change the flow name from Untitled to **Notify Problem Creator.**

1. Click **Save** and wait for the flow to be saved.

    ![A screenshot of the current flow](04/media/image8.png)

1. **Close** the flow designer browser window or tab.

1. Click **Done** on the popup window.

#### Task 2: Test the flow

In this task, you will test the notify problem creator flow.

1.  Make sure you are still on the [Power Apps maker portal](https://make.powerapps.com/) site and you are in the correct environment.

1.  Select **Apps**, and then select the **Company 311 Admin** Model-driven application. Click **Play**.

1.  Click **+ New**.

1.  Enter **Flow test** for **Title**, select **London Paddington** for **building**, enter **This is a flow test** for **Details**, and click **Save**.

1.  Scroll down and change the **Status Reason** value to **In Progress** and save again.

1.  Close the application browser window or tab.

1.  You should now be back to the [Power Apps maker portal](https://make.powerapps.com/)

1.  Select **Solutions** and click to open the **Company 311** solution.

1.  Locate and click to open the **Notify Problem Creator** flow you created.

1. You should see a succeeded flow run in the **28-day run history section**. Click to open the run.

    ![A Screenshot with an arrow pointing to the start date of the 28-day run history section](04/media/image9.png)

1. All the flow steps should have a **green** check mark.

1.  Open your personal Outlook account which you signed in for Task 1 Step 20.

1. You should get an email from the flow. Click to open the email.

1. The email should look like the image below.

    ![A screenshot of the email you should receive with the status of the problem, problem title, and its current status](04/media/image11.png)

### Exercise 2: Build escalation flow

In this exercise, you create add two new Columns to the problem report Table and create escalation flow.

#### Task 1: Add Columns

In this task, you add a new Columns to the problem report Table.

1.  Navigate to the [Power Apps maker portal](https://make.powerapps.com/) and make sure you are in the correct environment.

1.  Select **Solutions** and click to open the **Company 311** solution.

1.  Locate and click to open the **Problem Report** Table.

1.  Make sure you have the **Columns** tab selected and click **+ Add Column**.

1.  Enter **Estimated Cost** for **Display name**, select **Currency** for **Data type** and click **Done**.

1.  Click the **Save Table** located on bottom right of the screen.

1.  Select the **Forms** tab.

1.  Click to open the **Information** form of type **Main**.

1.  Add **Estimated Cost** Column to the form and place it below the **Status Reason** Column.

1. Add the **Assign to** Column and place it below the **Estimated Cost** Column.

1. The **Resolution details** section of the form should now look like the image below. Click **Save**.

    ![A Screenshot with a border around estimated cost and assign to placed correctly and an arrow pointing to the save button](04/media/image12.png)

1. Click on the **Back** button located on the top left of the screen.

1. Select **Solution**, click **Publish all customizations**, and wait for the publishing to complete.

#### Task 2: Build escalation flow

In this task, you will create the escalation flow.

1.  Navigate to the [Power Apps maker portal](https://make.powerapps.com/) and make sure you are in the correct environment.

1.  Select **Solutions** and click to open the **Company 311** solution.

1.  Click **+ New** and select **Cloud flow** and click on **Automate**.

1.  Search for **when a row is added** and select **When a row is added, modified, or deleted**  from **Microsoft Dataverse** connector.

1.  Select **Added or Modified** for **Change type**, select **Problem Reports** for **Table name**, select **Organization** for **Scope** and click **Show advanced options**.

1.  Enter **lh_estimatedcost,lh_assignto** for **Select columns** and click **Hide advanced options**.

1.  Click on the **… Menu** button of the trigger step and select **Rename**.

1.  Rename the trigger step **When a problem report is created or updated**.

1.  Click **+ New step**.

1. Search for **Condition** and Select **Condition** control.

1. Click to select the first **Choose a value** Column.

1. Go to the Dynamic content pane, search for **estimated** and select **Estimated Cost**.

    ![A screenshot of the dynamic content pane with the word estimated in the search bar](04/media/image13.png)

1. Select **is greater than** in the second field and enter **1000** in the third field.

1. Rename the condition step to **Check if cost is greater than 1000**.

1. Go to the **If yes** branch and click **Add an action**.

1. Search for **Get a row** and select **Get a row by ID** from **Microsoft Dataverse**.

1. Select **Users** for **Table name**.

1. Click to select the **Row ID** Column and select **Assign to (Value)** from the **Dynamic content** pane.

1. Click **Show advanced options**.

1. Enter **internalemailaddress** for **Select columns**.

1. Click **Hide advanced option**.

1. Rename the **Get a Row by ID** step **Get user**.

1. Click **Add and action**.

1. Search for **approval** and select **Start and wait for an approval**.

1. Select **Approve/Reject - Everyone must approve** for **Approval type**.

1. Enter **Cost approval required** for **Title**.

1. Click to select the **Assigned to** Column.

1. Go to the **Dynamic content** pane and select **Primary Email** from the **Get user** step.

1. Paste the markdown text below in the **Details** Column.

    > \#\# URGENT Approval Required
    >
    > This is \*\*very\*\* expensive item with the estimated cost of

1. Place your cursor after cost of, go to the Dynamic content pave, select the Expression tab, paste the expression below, and click **OK**.

    `formatNumber(triggerOutputs()?['body/lh_estimatedcost'], 'C2')`

    ![A Screenshot with an arrow pointing to the ok button in the expression tab under the pasted expression](04/media/image14.png)

1. Click **Add an action**.

1. Search for **condition** and select **Condition** control.

1. Click to select the first **Choose a value** Column.

1. Go to the **Dynamic content** pane, search for **Outcome** and select **Outcome**.

1. Select **equals to** in the second field and type **Reject** for value in the third field.

1. Go to the **If yes** branch and click **Add an action**.

1. Search for **update a Row** and select **Update a Row** from **Microsoft Dataverse**.

1. Select **Problem Reports** for **Table name**.

1. Click to select the **Row ID** Column.

1. Go to the **Dynamic content** pane, search for **problem report** and select **Problem Report**.

1. Click **Show advanced options**.

1. Click to select the **Resolution** Column, go to the **Dynamic content** pane and select **Response summary**.

1. Select **Won’t fix** for **Status Reason**.

1. Rename the step **Update problem report**.

1. Scroll up and rename the flow **Escalate Expense Approval**.

1. Click **Save**.

    ![A screenshot of the current flow](04/media/image15.png)

1. Close the flow designer browser window or tab.

1. Click **Done** on the popup.

#### Task 3: Test flow

In this task, you will test the escalation flow

1.  Navigate to the [Power Apps maker portal](https://make.powerapps.com/) and make sure you are in the correct environment.

1.  Select **Apps** and click to open the **Company 311 Admin** application.

1.  Click to open one of the **Problem Report** rows.

1.  Scroll down, enter **2500** for **Estimated Cost**, assign it to Odl id of yourself(for test purposes) and click **Save**.

1.  Navigate to [Power Automate](https://us.flow.microsoft.com/en-us/)

1.  Click **Sign in** and enter the username and password provided in the environment tab.

1.  Expand **Action Items** and select **Approvals**.

1.  You should see at least one approval in the received tab. Click to open the approval. It can take around 10-15 minutes for approvals to show up here on the first run.

    ![A Screenshot with an arrow pointing to the cost approval required request](04/media/image16.png)

1.  Select **Reject**, enter **We don't have the funds for this item** for **comment**, and click **Confirm**.

    ![A screenshot of the details of the request with the relevant text in each field](04/media/image17.png)

1.  Go back to the **Company 311 Admin** application.

1. Change the view to **My Reports** and click to open the same row you change the estimated cost.

1. The **Status Reason** should be set to **Won’t fix** and the **Resolution** should contain the details of Approver, Response, Request Date and Response Date.

1. Click **Save**, if you have not done so previously.

    ![A screenshot of the status reason and resolution matching the values and text you put into the request](04/media/image18.png)

### Exercise 3: Send approval requests as adaptive card in Microsoft Teams

In this exercise, you will setup a team in Microsoft Teams dedicated to the Company 311 applications. You will modify the flow to send the approval request as an adaptive card in Teams chat instead of an approval message.

* Task 1: Setup Company 311 Team
* Task 2: Modify flow to send adaptive card in Teams chat
* Task 3: Test adaptive card

#### Task 1: Setup Company 311 Team

In this task you will setup a Microsoft Teams team for the Lamna Healthcare Company, if you have not done so in previous exercises.

1.  Navigate to [Microsoft Teams](https://teams.microsoft.com) and sign in with the same credentials you have been using previously.

1.  Select **Use the web app instead** on the welcome screen.

    ![A screenshot of the Microsoft Teams landing page with a border around the use the web app instead button](04/media/image-5-teams.png)

1.  When the Microsoft Teams window opens, dismiss the welcome messages.

1.  On the bottom left corner, choose **Join or create a team**.

1.  Select **Create a team**.

    ![A screenshot with a box around the join or create a team button at the bottom of the window and a border around the create a team button](04/media/image-5-createteam.png)

1.  Press **From scratch**.

1.  Select **Public**.

1.  For the Team name choose **Company 311** and select **Create**.

1.  Select **Skip** adding members to Company 311.


#### Task 2: Modify flow to send adaptive card in Teams chat

In this task you will replace the approval sent by email with the adaptive card.

1. Locate **Start and wait for an approval** step created earlier in **Exercise 2, Task 2**.

1. Select **...** then select **Delete**.

1. Click **+** between the steps to insert a new step then select **Add an action**.

1. Search for **approval** and select **Create an approval**.

1. Select **Approve/Reject - Everyone must approve** for **Approval type**.

1. Enter **Cost approval required** for **Title**.

1. Click to select the **Assigned to** Column.

1. Go to the **Dynamic content** pane and select **Primary Email** from the **Get user** step.

1. Paste the markdown text below in the **Details** Column.

    > \*\*{title}\*\*
    >
    > 
    >
    > {details}
    >
    > 
    >
    > This is a \_very\_ expensive item with the estimated cost of

1. Select **{title}** placeholder, go to the **Dynamic content** pane, locate and select **Title** Column from **When a problem report is created or updated** step.

1. Select **{details}** placeholder, go to the **Dynamic content** pane, locate and select **Details** Column from **When a problem report is created or updated** step.

1. Place your cursor after **cost of** , go to the **Dynamic content** pane, select the **Expression** tab, paste the expression below, and click OK.

    `formatNumber(triggerOutputs()?['body/lh_estimatedcost'], 'C2')`

1. Your step should look like the following:

    ![A screenshot of the create an approval window with the following. Approval type as approve/reject - everyone must approve, title as cost approval required, assigned to primary email, details as title, details, some text and format number, item link, and item link description](04/media/image-5-create-approval.png)

1. Click **+** then select **Add an action**.

1. Search for **teams** and select **Post adaptive card in a chat or channel** action.

1. Select **Flow bot** for Post as and select **Chat with Flow bot** for Post in.

1. Click to select the **Recipient** field.

1. Go to the **Dynamic content** pane and select **Primary Email** from the **Get user** step.

1. Click to select **Adaptive Card** field.

1. Go to the **Dynamic content** pane and select **Teams Adaptive Card** from the **Create an approval** step.

1. Post adaptive card in chat or channel should look like the image below.

    ![A screenshot of the post adaptive card in a chat or channel pane](04/media/image-5-post-adaptive-card.png)

1. Select **+** then select **Add an action**.

1. Search for **approval** and select **Wait for an approval** action.

1. Select **Approval ID** Column.

1. Go to the **Dynamic content** pane and select **Approval ID** from the **Create an approval** step.

    ![A screenshot of the wait for an approval panel with approval ID in the approval ID field](04/media/image-5-wait-for-approval.png)

1. You now have replaced **Start and wait for an approval** step with the following:

    ![A screenshot of the current flow with: create an approval, post adaptive card in a chat or channel, and wait for an approval](04/media/image-5-replaced-approval.png)

1. Expand **Condition 2** step. The left side of the condition should be empty because it was referring the step which is now removed. 

1. Go to the **Dynamic content** pane, search for **outcome,** and select **Outcome** from **Wait for an approval** step. 

1. Locate **Update problem report** step under **If yes** branch.

1. Click **Show advanced options**.

1. Click to select the **Resolution** Column, go to the **Dynamic content** pane, and select **Response summary** from **Wait for an approval** step.

1. Click **Save**.

1. Close the flow designer browser window or tab.

1. Click **Done** on the popup.

#### Task 3: Test flow

In this task, you will test the escalation flow with the Teams and adaptive cards.

1.  Navigate to the [Power Apps maker portal](https://make.powerapps.com/) and make sure you are in the correct environment.

1.  Select **Apps** and click to open the **Company 311 Admin** application.

1.  Click to open one of the **Problem Report** Rows.

1.  Scroll down, enter any amount greater than **1000** for **Estimated Cost**, assign it to **yourself** (for test purposes) and click **Save**.

1.  Navigate to [Microsoft Teams](https://teams.microsoft.com)

1.  Select **Chat**.

1. You should see the Cost Approval Required Adaptive Card.

    ![A screen shot of the request for cost approval pane](04/media/image-5-sample-adaptive-card.png)  

1. Press **Reject** button and enter a comment of your choice in the Comments area, for example **The item is too expensive**.

1. Select **Submit**.  The card will become read-only.

    ![A screenshot of the request once you have rejected it](04/media/image-5-readonly-card.png)

1. Go back to the **Company 311 Admin** application.

1. Change the view to **My Reports** and click to open the same Row you change the estimated cost.

1. The **Status Reason** should be set to **Won’t fix** and the **Resolution** should contain the details of Approver, Response, Request Date and Response Date.

    ![A screenshot of the status reason and resolution matching the details of your response to the request](04/media/problemreportadaptivecard.png)

## **Discussion**

  - Would creating a bool Column for Approved/Rejected be better?
  - What are the pros and cons of using Microsoft Teams over regular email?

## **Bonus exercises**

  - Add ability for the users to subscribe to the reported problems and only notify if there is a subscription. 
  - Auto-subscribe creator of the problem report.
  - How to find out previous value of status reason?
  - Create your own adaptive card using [Adaptive Cards Designer](https://adaptivecards.io/designer/).
