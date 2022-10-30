# Exercise - Using IBM MQ and IBM Event Streams for near realtime data replication

In this lab you will use IBM MQ and IBM Event Streams to replicate data from a transactional database to a reporting database. The pattern used allows for seamless horizontal scaling to minimize the latency between the time the transaction is committed to the transactional database and when it is available to be queried in the reporting database.

The architecture of the solution you will build is shown below:

![Architecture diagram](images/architecture.png)

* The **portfolio** microservice sits at the center of the application. This microservice:

    * sends completed transactions to an IBM MQ queue.
    * calls the **trade-history** service to get aggregated historical trade data.

* The **Kafka Connect** source uses the Kafka Connect framework and an IBM MQ source to consume the transaction data from IBM MQ and sends it to a topic in IBM Event Streams. By scaling this service horizontally you can decrease the latency between the time the transaction is committed to the transactional database and when it is available to be queried in the reporting database,

* The **Kafka Connect** sink uses the Kafka Connect framework and a Mongodb sink to receive the transaction data from IBM Event Streams  and publishes it to the reporting database. By scaling this service horizontally you can decrease the latency between the time the transaction is committed to the transactional database and when it is available to be queried in the reporting database.

This lab is broken up into the following steps:

1. [Uninstall the TraderLite app](#step-1-uninstall-the-traderlite-app)

1. [Create a topic in the Event Streams Management Console](#step-2-create-a-topic-in-the-event-streams-management-console)

1. [Add messaging components to the Trader Lite app](#step-3-add-messaging-components-to-the-trader-lite-app)

1. [Generate some test data with the Trader Lite app](#step-4-generate-some-test-data-with-the-traderlite-app)

1. [Verify your trades have been sent as messages to IBM MQ](#step-5-view-messages-in-ibm-mq)

1. [Start Kafka Replication](#step-6-start-kafka-replication)

1. [Verify transaction data was replicated to the Trade History database](#step-7-verify-transaction-data-was-replicated-to-the-trade-history-database)

1. [Examine the messages sent to your Event Streams topic](#step-8-examine-the-messages-sent-to-your-event-streams-topic)

1. [Summary](#summary)


>**Note:** You can click on any image in the instructions below to zoom in and see more details. When you do that just click on your browser's back button to return to the previous state.

## Step 1: Uninstall the TraderLite  app
If you've completed the API Connect and/or the Salesforce integration labs then you will have the app running already. For this lab it's easier to install the app from  scratch so you can use the OpenShift GUI environment (as opposed to editing the yaml file of an existing instance) to select all the options needed for this app. If the app is *NOT* installed skip to **Step 2**.

1.1 Go to the OpenShift console of your workshop cluster. Select your  ***student001*** project. In the navigation on the left select **Installed Operators** in the **Operators** section and select the **TraderLite Operator**

  [![](images/traderlite-operator.png)](images/traderlite-operator.png)

1.2 Click on the **TraderLite app** tab

  [![](images/traderlite-crd.png)](images/traderlite-crd.png)

1.3 Click on the 3 periods to the right of the existing TraderLite CRD and select **Delete TraderLite** from the context menu.

  [![](images/select-traderlite-crd2.png)](images/select-traderlite-crd2.png)

1.4 In the navigation area on the left select **Pods** in the **Workloads** section. You should see that the *traderlite-xxxxx-yyyyy* pods are terminated.  

  > *Note: You can enter `traderlite` in the search by name input field to filter the pods.*

## Step 2: Create a topic in the Event Streams Management Console

2.1  Go to your Workshop Information page and click on the Event Streams component link. (**Note:** if you no longer have the Workshop Information page available see [these instructions](../pre-work/README.md)).

  [![](images/nav-to-es.png)](images/nav-to-es.png)

2.2 If prompted to login enter the credentials on the Workshop Information page

2.3 Click on the **Create a topic** tile

  [![](images/create-topic.png)](images/create-topic.png)

2.4 Name the topic `student001`. Click **Next**.

2.5 Leave the default for message retention and click **Next**.

2.6 Leave the default for replicas and click **Create topic**.

2.7 You should see your new topic listed.

  [![](images/new-topic.png)](images/new-topic.png)

## Step 3: Add messaging components to the Trader Lite app

In this section you will install the TraderLite app to start storing transactions as MQ messages, without setting up the KafkaConnect part that will move the transactions out of MQ, into Event Streams and then into MongoDB. This demonstrates how MQ can serve as a reliable store and forward buffer especially during temporary network disruption.

3.1 Go to the OpenShift console of your workshop cluster. Select your  ***student001*** project. 

3.2 Click on **Installed Operators** (in the **Operators** section) in the left navigation and then click on the **TraderLite Operator** in the list.

  [![](images/select-traderlite-operator.png)](images/select-traderlite-operator.png)

3.3 Click the **Create Instance** to start the installation of the TraderLite app.

  [![](images/traderlite-create-instance.png)](images/traderlite-create-instance.png)

3.4 Name the instance *traderlite*  

3.5 Set the following values:

   + Under **Kafka Access** select the **student001** topic

   + Enable **KafkaIntegration**

   + Under **Mqaccess** select the **STUDENT001.QUEUE** queue

  [![](images/traderlite-create-values.png)](images/traderlite-create-values.png)

3.6 Scroll down and click **Create**

3.7 In the left navigation select **Pods** (in the **Workloads** section) and then wait for all the TraderLite pods to have a status of **Running** and be in the **Ready** state.

> *Note: You will know the traderlite-xxxxx pods are  in a ready state when the `Ready` column shows `1/1`.*


## Step 4: Generate some test data with the TraderLite app

4.1 In your OpenShift console click on **Routes** in the left navigation under the **Networking** section and then click on the icon next to the url for the **tradr** app (the UI for TraderLite)

  [![](images/traderlite-run-tradr.png)](images/traderlite-run-tradr.png)

4.2 Log in using the username `stock` and the password `trader`

  [![](images/stock-trader-login.png)](images/stock-trader-login.png)

4.3 Click **Add Client** fill in the form. You must use valid email and phone number formats to avoid validation errors.

  [![](images/new-client.png)](images/new-client.png)

4.4 Click **Save**

4.5 Click on the **Portfolio ID** of the new client to see the details of the portfolio

  [![](images/new-portfolio.png)](images/new-portfolio.png)

4.6 Buy some shares of 2 or 3 different companies and then sell a portion of one of the shares you just bought. To buy shares, click the `Buy Stock` button, then select a company and enter a share amount. To sell shares, click the `Sell stock` button, then select the company symbol and enter the number of shares to sell.

  [![](images/a-few-trades.png)](images/a-few-trades.png)

>**Note:** Your ROI will be off because we are not yet replicating the historical trade data that goes in to the calculation of the ROI.

## Step 5: View messages in IBM MQ

5.1 Go to your Workshop Information page and click on the App Connect Designer component link. (**Note:** if you no longer have the Workshop Information page available see [these instructions](../pre-work/README.md)).

  [![](images/nav-to-mq.png)](images/nav-to-mq.png)

5.2 If prompted to login enter the credentials on the Workshop Information page.

5.3 Click on the **Manage QMTRADER** tile

  [![](images/manage-qmtrader-tile.png)](images/manager-qmtrader-tile.png)

5.4 Click on the **STUDENT001.QUEUE** queue.

  [![](images/trader-queue.png)](images/trader-queue.png)

5.5 You should see messages for the trades you just executed. The number of messages in the queue will vary based on the number of buy/sell transactions you performed in the previous steps.

  [![](images/mq-messages.png)](images/mq-messages.png)

5.6 Keep the browser tab with the MQ web interface open as you'll need it later in the lab.

## Step 6: Start Kafka Replication

In this section you will configure the TraderLite app to start moving the transaction data out of MQ, into Kafka and then into the MongoDB reporting database.

6.1 Go to the OpenShift console of your assigned cluster.  In the navigation on the left select **Installed Operators** and select the **TraderLite Operator**

6.2 Click on the **TraderLite app** tab

  [![](images/traderlite-crd.png)](images/traderlite-crd.png)

6.3 Click on the 3 periods to the right of the existing TraderLite CRD and select **Edit TraderLite** from the context menu.

6.4 Scroll down to line 432 and set **Kafka Connect enabled** to  **true** (leave all the other values unchanged).

  [![](images/kafka-connect-enabled.png)](images/kafka-connect-enabled.png)

6.5 Click **Save**.

6.6 In the navigation on the left select **Installed Operators** and select the **Event Streams** operator.

  [![](images/es-operator.png)](images/es-operator.png)

6.7 Click on the **All instances** tab and wait for the *mq-source* and *mongodb-sink* connectors to be in the *Ready* state before continuing.

  [![](images/kc-status.png)](images/kc-status.png)

6.8 Go back to the browser tab with the MQ Console and verify that all the messages have been consumed by the *mq-source* connector. *(Note: You may need to reload this browser tab to see that the messages have been consumed.)*

  [![](images/mq-empty.png)](images/mq-empty.png)

## Step 7: Verify transaction data was replicated to the Trade History database

7.1 Go to the OpenShift console of your workshop cluster.  In the navigation on the left select **Routes** in the **Networking** section.

7.2 Copy the link for the *trade-history* microservice and paste it into a new browser tab.

> **Note:** You will get a 404 (Not Found) message if you try to access this URL as is. This is because the *trade-history* microservice requires extra path information.

  [![](images/trade-history.png)](images/trade-history.png)

7.3 Append the string `/trades/1000` to the address you pasted - you should get back some JSON with the test transactions that you ran earlier.

  [![](images/trade-history2.png)](images/trade-history2.png)


## Step 8: Examine the messages sent to your Event Streams topic

8.1 Go to your Workshop Information page and click on the Event Streams component link. (**Note:** if you no longer have the Workshop Information page available see [these instructions](../pre-work/README.md)).

  [![](images/nav-to-es.png)](images/nav-to-es.png)

8.2 Click on the topics icon

  [![](images/topics-icon.png)](images/topics-icon.png)

8.3 Click on topic **student001**

  [![](images/topic-name.png)](images/topic-name.png)

8.4 Select a message to see it's details

  [![](images/message-details.png)](images/message-details.png)


## Summary

Congratulations ! You successfully completed the following key steps in this lab:

* Configured the Trader Lite app to use MQ
* Deploy Kafka Connect with IBM Event Streams
* Generated transactions in the Trader Lite app and verified that the data is being replicated via MQ and Event Streams
