# Exercise: Using IBM Event Streams for near real-time data replication

In this lab you will use IBM MQ and IBM Event Streams to replicate data from  a transactional database to a reporting database. The pattern used allows for seamless horizontal scaling to minimize the latency between the  time the transaction is committed to the transactional database and when it is available to be queried in the reporting database.

* The **event-producer** microservice consumes the  transaction data from IBM MQ and sends it to a topic in Event Streams. By scaling this service horizontally you can decrease the latency between the  time the transaction is committed to the transactional database and when it is available to be queried in the reporting database,

* The **event-consumer** microservice receives the transaction data from  Event Streams and calls the  **trade-history** service to publish the data to the reporting database. By scaling this service horizontally you can decrease the latency between the  time the transaction is committed to the transactional database and when it is available to be queried in the reporting database.

## Steps

1. [Create a topic in the Event Streams Management Console](#step-1-create-a-topic-in-the-event-streams-management-console)

1. [Get credentials for your Event Streams topic](#step-2-get-credentials-for-your-event-streams-topic)

1. [Upload the Java truststore to the IBM Cloud Shell](#step-3-upload-the-java-truststore-to-the-ibm-cloud-shell)

1. [Add messaging components to the Stock Trader app](#step-4-add-messaging-components-to-the-stock-trader-app)

1. [Generate some test data with the Stock Trader app](#step-5-generate-some-test-data-with-the-stock-trader-app)

1. [Verify transaction data was replicated to the Trade History database](#step-6-verify-transaction-data-was-replicated-to-the-trade-history-database)

1. [Examine the messages sent to your Event Streams topic](#step-7-examine-the-messages-sent-to-your-event-streams-topic)

1. [Summary](#summary)

### Step 1: Create a topic in the Event Streams Management Console

1.1 Access the Event Streams Management Console using the URL provided to you by your instructors.

1.2 Sign in with the credentials provided to you by your instructors.

1.3 Click on the **Create a topic** tile

![Create a topic](images/create-topic.png)

1.4 Name the topic `stocktrader-user???` where `user???` is your assigned student id. For example if your student id is `user002` then name the topic `stocktrader-user002`. Click **Next**.

1.5 Leave the default for the number of partitions and click **Next**.

1.6 Leave the default for message retention and click **Next**.

1.7 Change the Replication factor to 1 and click **Create topic**.

![Replication factor](images/replication-factor.png)

1.8 You should see your new topic listed.

### Step 2: Get credentials for your Event Streams topic

2.1 From the Event Streams Management Console click **Connect to this cluster**.

![Connect to cluster](images/connect-to-cluster.png)

2.2 Click on the icon next to the **Bootstrap server** hostname to copy it to the clipboard.

![Copy bootstrap hostname](images/copy-bootstrap-hostname.png)

2.3 Create a local file and paste the bootstrap server hostname into it.  You'll need this later to connect the Stock Trader application to your Event Streams topic.

2.4 Click **Generate API key**

![Generate API key](images/generate-api-key.png)

2.5 Name the application `stocktrader-user???` where `user???` is your assigned student id. For example if your student id is `user002` then name the application`stocktrader-user002`.

2.6 Select **Produce and consume** for the capabilities of the API Key. Click **Next**.

![API Key capabilities](images/api-key-capabilities.png)

2.7 Enter the name of your topic e.g. `stocktrader-user002`. Click **Next**.

2.8 Click **Generate API key**

2.9 Click **Copy API key** and paste the API Key into the same file that you used to save the bootstrap hostname.

![Copy API Key](images/copy-api-key.png)

2.10 Click on the download icon to download the Java truststore needed for Java applications to connect to Event Streams

![Download Java truststore](images/download-java-truststore.png)

2.11 This will prompt you to save the file **es-cert.jks** locally. Save the file.

2.12 Keep this browser tab open for later on in the lab.

### Step 3: Upload the Java truststore to the IBM Cloud Shell

3.1 In the IBM Cloud Shell browser tab, click on the upload icon

![Upload icon](images/upload-icon.png)

3.2 Select the file **es-cert.jks** that you downloaded in the previous step and follow the prompts to upload it to the IBM Cloud Shell.

### Step 4: Add messaging components to the Stock Trader app

4.1 you haven't previously cloned the Github repo with the Stock Trader app deployment artifacts, run the following command in the IBM Cloud Shell

```bash
git clone https://github.com/IBMStockTraderLite/stocktrader-cp4i.git
```

4.2 Go to the directory required to run the setup scripts

```bash
cd stocktrader-cp4i/scripts
```

4.3 Run the following command, substituting the Bootstrap hostname and API Key that you saved earlier. Note that the third parameter is the Java truststore file that you just uploaded.

```bash
./addMessaging.sh [BOOTSTRAP SERVER HOSTNAME] [YOUR API KEY] ../../es-cert.jks
```

The output should look like the following:

![Add messaging script](images/add-messaging-script.png)

4.4 Wait for all the pods to start. Run the following command repeatedly until all the pods are in the *Ready* state as shown below

```bash
oc get pods | grep -v deploy
```

![Pods running](images/pods-running.png)

### Step 5: Generate some test data with the Stock Trader app

5.1 From the command line run the following script:

```bash
./showTradrURL.sh
```

5.2 Copy the URL that is output and access it with your browser

5.3 Log in using the username `stock` and the password `trader`

5.4 Click on **Add Client**

![Add Client](images/add-client.png)

5.5 Click on the Portfolio ID of the added client

![Portfolio Details](images/portfolio-id.png)

5.6 Click **Buy Stock**. Select a company and enter the number of shares. Click **Buy shares**.

5.7 Do 2 or 3 more transactions (either buy or sell).

![Buy or sell](images/buying-selling.png)

### Step 6: Verify transaction data was replicated to the Trade History database

6.1 From the  IBM Cloud Shell terminal, run the following command to show the Trader History service URL for listing trade data

```bash
./showHistoryURL.sh
```

6.2 Copy the URL that is output and access it with your browser

6.3 Verify that trade data in JSON format is shown and the **xactionsrc** fields is set to `Event Streams` indicating that this data was propagated to the Trade History database via IBM MQ and Event Streams.

![Trade History data](images/trade-history-data.png)

### Step 7: Examine the messages sent to your Event Streams topic

In this step you'll examine the data sent to your Event Streams topic.

7.1 From the Event Streams Management Console click on the topics icon

![Topics icon](images/topics-icon.png)

7.2 Click on your topic name

![Topic Name](images/topic-name.png)

7.3 Click on **Messages** and then select an individual message to see the  details.

![Messages](images/topic-messages.png)

7.4 Details of the message you selected should appear at the right

![Message Details](images/message-details.png)

## Summary

Congratulations ! You successfully completed the following key steps in this lab:

* Created an Event Streams topic
* Retrieved all the credentials needed to access the topic from  an application .
* Configured the Stock Trader app to use your topic
* Generated transactions in the Stock Trader app and verified that the data is being replicated via Event Streams.
* Examined messages sent to your topic in the Event Streams Management console.
