# Exercise 4: Use App Connect to easily integrate an external data source

* Go to App Connect catalog, create a connection to the running MQ service by providing the usual credentials
* Provision a Cloudant DB service on IBM Cloud
* Create a flow that reads new messages from MQ (subscribe to a queue) and backs them up to Cloudant (new document)
* Launch Cloudant console to ensure the messages are there
