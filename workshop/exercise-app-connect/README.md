# Exercise: Use App Connect to sync Salesforce data

In this lab you will create an App Connect flow  to push client data from the **IBM Stock Trader Lite** app to Salesforce. This will occur whenever a user of the  **IBM Stock Trader Lite** app creates a new portfolio for a new client. Basic information about the client is stored in the  **IBM Stock Trader Lite** app and more detailed info is stored in Salesforce.

The architecture of the app is shown below:

![Architecture diagram](images/architecture.png)

The **portfolio** microservice invokes the REST endpoint of the new flow whenever a new client is created and this results in a new contact being created for the new user in Salesforce.

## Steps

1. [Sign up for Salesforce Developer Edition](#section-1-sign-up-for-salesforce-developer-edition)

1. [Create a Salesforce Connected App](#section-2-create-a-salesforce-connected-app)

1. [Setup connectivity to Salesforce in App Connect Designer](#section-3-setup-connectivity-to-salesforce-in-app-connect-designer)

1. [Create the flow in App Connect Designer](#section-4-create-the-flow-in-app-connect-designer)

1. [Save your Salesforce credentials as an OpenShift secret](#section-5-save-your-salesforce-credentials-as-an-openshift-secret)

1. [Create an Integration Server instance and deploy your flow](#section-6-create-an-integration-server-instance-and-deploy-your-flow)

1. [Test your App Connect Flow](#section-7-test-your-app-connect-flow)

1. [Summary](#summary)

### Section 1: Sign up for Salesforce Developer Edition

1.1 In your browser go to the following URL to sign up for Salesforce Developer Edition - a full featured version of the Salesforce Lightning Platform. **Note:** If you already have access to  the Salesforce Developer Edition or a paid version of Salesforce, skip to **Step 2**.

   [Salesforce Developer Edition signup](https://developer.salesforce.com/signup)

1.2 Enter the required information and follow the prompts to complete the signup process.

### Section 2: Create a Salesforce Connected App

In this section you'll create a *Connected App* in Salesforce so that the App Connect flow that you create later will be able to access your Salesforce data.

2.1 Login to [Salesforce](https://www.salesforce.com)

2.2 Click on your avatar (top right) and select **Switch to Salesforce Classic** from the context menu.

![Salesforce login](images/sflogin.png)

2.3 Click on **Setup**.

![Salesforce setup](images/sfsetup.png)

2.4 In the left navigation area, scroll down to the **Build** section, expand **Create** and click on **Apps**

![Create App](images/sfcreateapp.png)

2.5 Scroll down to the **Connected Apps** section and click on **New**

![Connected Apps](images/sfconnectedapp.png)

2.6 Fill in the required values for a new Connected App

   a. Set the **Connected App Name** property to `IBM App Connect`

   b. Set the **API Name** to `IBM_App_Connect`

   c. Enter your email address as the **Contact Email**

![New Connected App](images/sfnewconnectedapp.png)

2.7 Configure the OAuth settings

* Select the **Enable OAuth Setting** check box.

* Set the **Callback URL** to `https://www.ibm.com`

* Under **Selected OAuth Scopes** select  **Access and manage your data (api)** and click the arrow under the **Add** label

![OAuth settings](images/sfoauth.png)

2.8 Scroll down to the bottom of the web page and click **Save**

2.9 Copy the **Consumer Key** and **Consumer Secret** to a text file.  You will need them later to connect to Salesforce

2.10 Check your email for the email address that you use as your Salesforce username. You should have received an email from  Salesforce with the subject **Your new Salesforce security token**. Copy the **Security Token** to the same text file that you used for the  **Consumer Key** and **Consumer Secret**

![Security token](images/sfsectoken.png)

### Section 3: Setup connectivity to Salesforce in App Connect Designer

App Connect Designer is a component of Cloud Pak for Integration that provides an authoring environment in which you can create, test, and share flows for an API. You can share your flows by using the export and import functions, or by adding them to an Asset Repository for reuse.

3.1 In a new browser tab open the URL for **App Connect Designer**. Sign in with the credentials assigned to you by your instructors. **Note** You will be sharing the IBM Cloud Pak for Integration cluster with other students so  you will see their work  as you navigate through he tool. You will use a naming scheme for the artifacts that you create that starts with the username assigned to you by the instructors (e.g. *user005*).

3.2 Click the **Settings** icon and then select **Catalog**

 ![App Designer Catalog](images/appdesignercatalog.png)

3.3 Expand **Salesforce** .If there are existing connections shown in a drop down list  select **Add a new account ...** from that list.

 ![New Salesforce Connection](images/addanewaccount.png)

   **Note** If there are no existing connection shown click on **Connect**

3.4 Enter the following values referring to the text file from  the previous section where you saved your Salesforce credentials.

* For the **Login URL** enter `https://login.salesforce.com`

* For the **Username** enter the email you use to login to Salesforce

* For the **Password** enter the password you use to login to Salesforce. Then append the value of the  **Security Token**  (that you saved in the previous section) to the password. For example if your password is `foo` and your security token is `bar` you would enter `foobar` into the password field.

* For the **Client Id** copy and past the value of the  **Consumer Key** (that you saved in the previous section).

* For the **Client Secret** copy and past the value of the  **Consumer Secret** (that you saved in the previous section).

![Filled in Salesforce Connection](images/sfconnectionform.png)

3.5 Click on **Connect**. The connection will be given a default name of the  form *Account n*. Click on the 3 vertical dots next to the connection name and select **Rename Account**

![Rename account](images/renameaccount.png)

3.6 Enter the username you used to login to App Connect Designer as the new account name (e.g. *user005*) and click on **Rename Account**.

![Rename account](images/renameaccount2.png)

### Section 4: Create the flow in App Connect Designer

4.1 In App Connect Designer, click the **Settings** icon and then select **Dashboard**

![Dashboard](images/dashboard.png)

4.2 Click  **New** and select **Flows for an API**

![New Flow](images/newflow.png)

4.3 Name the flow `usernnnsf`  where *usernnn* is the  username assigned to you (e.g. *user005sf*).  Name the model `Client`

![Flow name](images/flowname.png)

4.4 Click **Create Model**

4.5 Next you will add the properties of the input data for your flow. *Note:* Name them exactly as instructed (including matching case) so that your flow will work with the *Stock Trader Lite* app.

* Enter `ClientId` as the first property and then click **Add property +**

* Enter `FirstName` as the next property and then click **Add property +**

* Enter `LastName` as the next property and then click **Add property +**

* Enter `Email` as the next property

   When you're done the screen should look like the following:

 ![Model properties](images/modelproperties.png)

4.6 Click  **Operations** and then select **Create Client**

 ![Operation](images/operation.png)

4.7 Click **Implement flow**

4.8 Click on the **+** icon.

* Scroll down to Salesforce

* Select your account from the dropdown

* Expand **Contacts**

* Click **Create Contact**

 ![Implement flow](images/implementflow1.png)

4.9 Next you'll map the properties from  your model to the Salesforce Contact properties. Click in the text box for the  **Account Id** property and then click on the icon just to the right of the field. Select the **ClientId** property of your model

![Map Client Id](images/mapclientid.png)

4.10 The remaining properties have the  same names as their  Salesforce equivalents so click on **Auto match fields** to complete the mapping

![Auto match fields](images/automatchfields.png)

4.11 Click on **Response** to configure what will be returned by the flow. Click in the text box for the **Client Id** property and then click on the icon just to the right of the field. Select the **Contact Id** property of the Salesforce contact.

![Return data](images/responsedata.png)

4.12 Next you'll test the flow to make sure it works. Click on the middle part of the flow and then click on the edit icon.

![Edit test parameters](images/testparams.png)

4.13 Click on **Request body parameters** and then edit the imput parameters that will be used in the test.

* Set the **Client Id** to blank. This value will be generated by Salesforce and returned.

* Enter a **FirstName** value.

* Enter a **LastName** value.

* Enter an **Email** in a valid email format

![Enter test parameters](images/edittestparams.png)

4.14 Click the test icon. Verify that the operation returns a 200 HTTP status code.

![Test status](images/teststatus.png)

4.15 Click **View details** to see the raw data returned from  the call to Salesforce (note this is not the same as the data returned by the flow which you defined in the **Response** stage of the flow). Click **Done**.

4.16 Next you'll export the flow so it can deployed in an Integration Server instance. Click the **Settings** icon and then select **Dashboard**. Click the 3 vertical dots on the tile for your new flow and select **Export ...** from the context menu.

![Export flow](images/exportflow.png)

4.17 Select **Export for integration server (BAR)** and click **Export**

4.18 Save the file to a folder of your choosing keeping the name that was pre-filled for you. (eg *user005sf.bar*).

### Section 5: Save your Salesforce credentials as an OpenShift secret

To deploy an Integration Server for your flow, you need to create a Kubernetes secret with your Salesforce credentials in the  OpenShift cluster running Cloud Pak for Integration.  We've created a helper app for you to do this.

5.1 In a separate browser tab, launch  the  helper app using the URL provided to you by the instructor. When prompted login using your workshop credentials

5.2 Enter the Salesforce authentication info requested. Note that in this UI the Salesforce password and Security Token are entered in separate fields.

![Save Salesforce credentials](images/savesfcreds.png)

5.3 Click **Save credentials**.  You should see a message like the following indicating the a secret was created.

![Save response](images/saveresponse.png)

### Section 6: Create an Integration Server instance and deploy your flow

In this Step you'll create an Integration Server instance and deploy your flow to it.

6.1 In a separate browser tab bring up the App Connect Dashboard using the URL provided to you by your instructors.  If prompted enter your user credentials. Click **Create server**

![Assemble](images/dashboardui.png)

6.2 Click **Add a BAR file** and select the file you exported at the end of the previous section.

![Continue](images/continue.png)

6.3 Click **Continue**.

6.4 Click **Next** (**Note:** you'll use a helper app to deploy the flow configuration so no need to download the config package).

6.5 Select **Designer** as the type of Integration that you want to run and click **Next**

![Integration Type](images/integrationtype.png)

6.6 Change the setting for **Show everything** to **ON**.

![Show everything](images/showeverything.png)

6.7 Enter the following settings:

* In the **Details** section for the **Name** enter `usernnsf` where *usernn* is the username of your credentials (e.g. *user005sf**)

* In the **Details** section for  **IBM App Connect Designer flows** select **Enabled for local connectors only**

* In the **Integration Server** section for **Name of the secret that contains the server configuration** enter `usernn-sf-connect` where *usernn* is the username of your credentials (e.g. *user005-sf-connect**)

* In the **Configuration for deployments** section change the **Replica count** to 1

The top half of the dialog should look like the following:

![Top Half](images/tophalf.png)

The bottom half of the dialog should look like the following:

![Bottom Half](images/bottomhalf.png)

6.8 Click **Create**. The status of the server will be eventually shown. Wait until the server status shows as **Started**. Note you may have to refresh the page to see the status change.

![Activate API](images/serverstarted.png)

### Section 7: Test your App Connect Flow

7.1 In the App Connect Dashboard click on the tile for your new server

![Running server](images/servertile.png)

7.2 Click on the API tile to see the details of the flow's API

![API tile](images/apitile.png)

7.3 You should see the details of your flow's API

![API Details](images/apidetails.png)

7.4 Copy the **REST API Base URL** to the clipboard

7.5 In a new browser tab launch the testing tool we have provided for the lab using the  URL given to you by your instructors.

7.6 Paste the  **REST API Base URL** to the field labelled **Flow Base URL**

![Test tool](images/testtool.png)

7.7 Click **Test flow**

7.8 The results of the API call will be displayed. Verify that the HTTP Status code is `201` and the ID of the new Salesforce contact is returned.

![Test results](images/testresults.png)

7.9 In a new browser tab login to Salesforce

7.10 Click on the **+** icon and then on **Contacts**

![Salesforce contacts](images/contacts.png)

7.11 Verify that the new contact you created via your test is there

![Verify contact](images/verifycontact.png)

## Summary

Congratulations ! You successfully completed the following key tasks in this lab:

* Connected to Salesforce
* Created an App Connect designer flow to push client data to Salesforce contacts.
* Deployed the flow as an Integration Server in App Connect Dashboard
* Configured a  ClientID/API Key  for security set up a proxy to the existing API.
* Tested the flow with new data.
