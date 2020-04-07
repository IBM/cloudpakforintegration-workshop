# Exercise 2: Add an API Management point to the application

[re-write everything below this line]

In this exercise, we will show how to ... :

When you have completed this exercise, you will understand how to

* Created an API by importing an OpenAPI definition for an existing REST service.
* Configured a ClientID/API Key for security set up a proxy to the existing API.
* Tested the API in the API Connect developer toolkit.

## Steps

1. [Download the OpenAPI definition file for the external Stock Quote service](#step-1-download-the-openapi-definition-file-for-the-external-stock-quote-service)

1. [Import the OpenAPI definition file into API Manager](#step-2-import-the-openapi-definition-file-into-api-manager)

1. [Configure the API](#step-3-configure-the-api)

1. [Test the API](#step-4-test-the-api)

1. [Create a new OpenShift project for the Stock Trader app](#step-5-create-a-new-openshift-project-for-the-stock-trader-application)

1. [Prepare for Installation](#step-6-prepare-for-installation)

1. [Install the Stock Trader app](#step-7-install-the-stock-trader-app)

1. [Verify that the Stock Trader app is calling your API successfully](#step-8-verify-that-the-stock-trader-app-is-calling-your-api-successfully)

1. [Summary](#summary)

## Step 1: Download the OpenAPI definition file for the external Stock Quote service

1.1 In your browser right click on  the following link, right click and select **Save Link As ...** from the context menu. Save the file *stock-quote-api.yaml* to  your local system.

   [stock-quote-api.yaml](https://raw.githubusercontent.com/IBMStockTraderLite/stocktrader-cp4I/master/apic/stock-quote-api.yaml)

## Step 2: Import the OpenAPI definition file into API Manager

2.1 Go to the browser tab with the API Manager Portal and click on the **Develop APIs and Products tile**

   ![Develop APIs and Products tile](images/api-manager.png)

2.2 Click **ADD->API**

  ![Add API](images/add-api.png)

2.3 On the next screen select **Existing OpenAPI** under **Import** and then click **Next**.

  ![Existing OpenAPI](images/existing-api.png)

2.4 Now choose **stock-quote-api.yaml** fromyour local file system and click **Next**.

  ![Choose file](images/choose-file.png)

2.5 **Do not** select **Activate API**. Click **Next**

  ![Choose file](images/activate-api.png)

2.6 The API should be imported successfully as shown below. Click **Edit API**.

  ![Edit API](images/edit-api.png)

## Step 3: Configure the API

After importing the existing API, the first step is to configure basic security before exposing it to other developers. By creating a client key  you are able to identify the app using the services. Next, we will define the backend endpoints where the API is actually running. API Connect supports pointing to multiple backend endpoints to match your multiple build stage environments.

3.1 In the Edit API screen click **Security Definitions**

3.2 In the **Security Definition** section, click the **Add** button on the right. This will open a new view titled **API Security Definition**.

3.2 In the **Name** field, type `client-id`.

3.3 Under **Type**, choose **API Key**. This will reveal additional settings.

3.4 For **Located In** choose **Header**. For **Key Type** choose **Client ID**. Your screen should look like the image below.

  ![Edit API complete](images/edit-api-complete.png)

3.5 Click the **Save** button to return to the **Security Definitions** section.

3.6 Click **Security** in the left menu. Click **Add**. Select the **client-id** as shown below and then click **Save**.

  ![Security](images/security.png)

3.7 Next you'll the define the endpoint for the external API. Click on **Properties** in the left menu.

3.8 Click on the **target-url** property. Click **Add**.

3.9 Choose the **sandbox** catalog and for the URL copy and paste the following URL:

    https://stock-trader-quote.us-south.cf.appdomain.cloud

   ![Target URL](images/target-url.png)

3.10 Click **Save** to complete the configuration.

## Step 4: Test the API

In the API designer, you have the ability to test the API immediately after creation in the **Assemble** view.

4.1 On the top Navigation, click **Assemble**.

  ![Assemble](images/assemble.png)

4.2 Click **proxy** in the flow designer. Note the window on the right with the configuration. It calls the **target-url** with the same request path sent to the API Connect endpoint.

  ![Proxy](images/proxy.png)

4.3 Click the play icon as indicated in the image below.

  ![Play icon](images/play-icon.png)

4.4 Click **Activate API** to publish the API to the gateway for testing.

  ![Activate API](images/activate-for-test.png)

4.5 After the API is published, your screen should look like the image below.

  ![API Published](images/api-published.png)

4.6 Under **Operation** choose get **/stock-quote/djia**.

4.7 Note that your **client-id** is prefilled for you.

4.8 Scroll all the way to the bottom of the browser window and click **Invoke**.

  ![Invoke API](images/invoke-api.png)

4.9 If this is the first test of the API, you may  see a certificate exception. Simply click on the URL and choose the option to proceed.

4.10 Go back to the test view and click **Invoke** again.

4.11 Now you should see a Response section with Status code 200 OK and the Body displaying the details of the Dow Industrial average.

  ![Return from API call](images/response.png)

4.12 Scroll up in the test view until you see the **Client ID**. Copy the value to to a local text file so it can be used in the Stock Trader application later (**Note:** this is a shortcut to the regular process of publishing the API and then subscribing to it as a consumer).

  ![Copy Client ID](images/client-id.png)

4.13 Next we'll get the endpoint for your API. Click on the **Home** icon (top left) and then click on the **Manage Catalogs** tile.

  ![Manage Catalogs](images/manage-catalogs.png)

4.14 Click on the **Sandbox** tile.

4.15 Click on the **Settings** icon and then on **API Endpoints**. Copy the gateway URL and put it in the same file that you used for the **Client ID**

  ![Gateway URL](images/gateway-url.png)

## Step 5: Create a new OpenShift project for the Stock Trader application

5.1 In the IBM Cloud Shell set an environment variable for the  *studentid* assigned to you by the instructors (e.g. **user001**)

```bash
export STUDENTID=user???
```

5.2 Create a new OpenShift project

```bash
oc new-project trader-$STUDENTID
```

## Step 6: Prepare for installation

Like a typical  Kubernetes app, Stock Trader use secrets to  store sensitive data  needed by one  or more microservices to access external  services and other microservices. You'll run a script to store your API Connect endpoint and apikey as secrets that the Stock Trader application references to get this information.

6.1 From the  IBM Cloud Shell terminal

```bash
git clone https://github.com/IBMStockTraderLite/stocktrader-cp4i.git
```

6.2 Go to the directory required to run the setup scripts

```bash
cd stocktrader-cp4i/scripts
```

6.3 Run the following command, substituting your API Connect endpoint URL and API Key that saved previously.

```bash
./setupAPIConnectAccess.sh [YOUR API CONNECT EXTERNAL URL] [YOUR API KEY]
```

The output should look like the following

![Setup API Connect Access](images/api-connect-access.png)

## Step 7: Install the Stock Trader app

You'll install Stock Trader using an OpenShift template.

7.1 Run the following script

```bash
./initialInstall.sh
```
7.2 Verify that the output looks like the following:

![Initial install](images/initial-install.png)

7.3 Wait for all the pods to start. Run the following command repeatedly until all the pods are in the *Ready* state as shown below

```bash
oc get pods | grep -v deploy
```

![Pods running](images/pods-running.png)

7.4 Initialize the database pods by running the following command

```bash
./createDbTables.sh
```

![Create db tables](images/create-db-tables.png)

## Step 8: Verify that the Stock Trader app is calling your API successfully

You will verify the configuration that you created that points at the API you created in API Connect.

8.1 From the command line run the following script:

```bash
./showTradrURL.sh
```

8.2 Copy the URL that is output and access it with your browser

8.3 Log in using the username `stock` and the password `trader`

  ![Stock Trader Login](images/stock-trader-login.png)

8.4 If the DJIA summary has data then you know that the API you created in API Connect is working !

  ![DJIA Summary Working](images/djia-success.png)
