# Exercise: Publish your API, using API Connect

In this exercise, we will show how to use API Connect to publish API.

When you have completed this exercise, you will understand how to

* Created a managed API endpoint, by importing an OpenAPI definition for an existing REST service.
* Configured a ClientID/API Key for security set up a proxy to the existing API.
* Tested the API in the API Connect developer toolkit.

## Steps

1. [Download the OpenAPI definition file](#1-download-the-openapi-definition-file)
1. [Create a Topology](#2-create-a-topology)
1. [Create an Organization](#3-create-an-organization)
1. [Add a Gateway to the Catalog](#4-add-a-gateway-to-the-catalog)
1. [Import the OpenAPI definition file](#5-import-the-openapi-definition-file)
1. [Configure the API](#6-configure-the-api)
1. [Test the API](#7-test-the-api)
1. [Summary](#summary)

### 1. Download the OpenAPI definition file

In your browser right click on the OpenAPI document link and select **Save Link As ...** from the context menu.

![Download OpenAPI Spec](images/download-api.png)

### 2. Create a Topology

From the hamburger menu click on the API Connect service, right click the kebab menu, and click **Manage**.

![Manage API Connect](images/manage-api-connect.png)

Click **Configure Topology**.

![Configure Topology](images/configure-topology.png)

Click **Register Service**.

![Register Service](images/register-service.png)

Click **DataPower Gateway**.

![Create Gateway](images/create-dp-gateway.png)

In the config menu...

* **Details**
  * *Title*: `Gateway 1`
* **Management Endpoint**
  * *Endpoint*: `https://gws.icp-proxy.apps.demo.ibmdte.net`
* **API Invocation Endpoint**
  * *Endpoint*: `https://gwy.icp-proxy.apps.demo.ibmdte.net`

![Gateway Endpoints](images/gateway-endpoints.png)

Click **Save** on the bottom of the page.

### 3. Create an Organization

From the hamburger menu click on the API Connect service, right click the kebab menu, and click **Manage**.

![Manage API Connect](images/manage-api-connect.png)

Click **Manage Organizations**.

![Manage Organization](images/manage-organization.png)

* Click on **Add** and choose **Create organization**

![Create Organization](images/create-org.png)

In the config menu...

* **Provider Organization**
  * *Title*: `Org 1`
* **Owner**
  * *User registry*: `API Manager Local User Registry`
  * Create a new user by giving it a `username`, `password`, `first name`, and `email`.

![Organization Details](images/org-details.png)

* Click **Create**

> **NOTE** You may see an error message `504 Gateway Time-out`, but that's OK.

### 4. Add a Gateway to the Catalog

In a new browser tab open the **Cloud Pak for Integration** tab and under **View Instances** click on the link for **API Connect**.

![Launch API Connect](images/cp4i-dashboard-api-connect.png)

You will be prompted to login, choose to login with **API Manager Local User Regitry**, and use the new username and password you just created.

![Manage API Connect](images/apic-login.png)

Click on **Manage Catalogs**.

![Manage Catalogs](images/manage-catalog.png)

Click on the **Sandbox** tile.

![Edit Sandbox Catalog](images/sandbox-catalog.png)

From the **Settings** (gear icon), choose the **Gateway Services** menu option, and add `Gateway-1` as the default.

![Add Gateway Services](images/add-gateway-services.png)

### 5. Import the OpenAPI definition file

In a new browser tab open the **Cloud Pak for Integration** tab and under **View Instances** click on the link for **API Connect**.

![Launch API Connect](images/cp4i-dashboard-api-connect.png)

Click on the **Develop APIs and Products tile**

![Develop APIs and Products tile](images/api-manager.png)

Click **ADD->API**

![Add API](images/add-api.png)

On the next screen select **Existing OpenAPI** under **Import** and then click **Next**.

![Existing OpenAPI](images/existing-api.png)

Now choose **user001sf.json** from your local file system and click **Next**.

![Choose file](images/choose-file.png)

Choose the default values in the next few panels. You'll reach the **Summary** panel and the API should be imported successfully as shown below. Click **Edit API**.

![Edit API](images/edit-api.png)

### 6. Configure the API

After importing the API you'll be given an opporunity to add or modify existing properties

![API Overview](images/api-overview.png)

Click **Save** to complete the configuration.

### 7. Test the API

In the API designer, you have the ability to test the API immediately after creation in the **Assemble** view.

On the top Navigation, click **Assemble**.

![Assemble](images/assemble.png)

Click **proxy** in the flow designer. Note the window on the right with the configuration. It calls the **target-url** with the same request path sent to the API Connect endpoint. Ensure the **URL** is set to `$(target-url)$(request.path)`.

![Proxy](images/proxy.png)

Click the play icon as indicated in the image below.

![Play icon](images/play-icon.png)

Click **Activate API** to publish the API to the gateway for testing.

![Activate API](images/activate-for-test.png)

After the API is published, find the **Operation** field and choose **post /Client**. Note that your **Client ID** is prefilled for you.

![Choose a POST operation](images/operation.png)

Choose to **Generate** data for a request and **REMOVE the Client ID**. Scroll all the way to the bottom of the browser window and click **Invoke**.

![Invoke API](images/invoke.png)

If this is the first test of the API, you may see a certificate exception. Simply click on the URL and choose the option to proceed.

Go back to the test view and click **Invoke** again.

Now you should see a Response section with status code `201 Created` and the body displaying the new Salesforce client ID.

![Return from API call](images/results.png)

## Summary

**Congratulations**! You successfully completed this part of the lab!
