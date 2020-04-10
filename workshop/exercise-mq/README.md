# Exercise: Integrate MQ with

In this task, you navigate to the MQ Console and check the MQ configuration. You create the queue which accepts the resulting data from the call to the "NEWORDER" API.

## Steps

Launch the MQ service. *If a security warning is see, add an exception and proceed.*

![Launch MQ](images/cp4i-dashboard-mq.png)

In the MQ Console page you should have a single queue manager named `mq1`. If you see queues on `mq1` skip ahead, otherwise, click the *Add Widget* button.

![Add a Widget](images/mq-console-add-widget.png)

Click *Queues* to create a queue widget. You can add different widgets into MQ Console, including MQ objects such as: charts, queues, topics, listeners, channels, etc.

![Add a Queue Widget](images/add-queue-widget.png)

From the new Queue Widget, click the *Create(+)* button to create a queue for the `mq1` queue manager.

![Start creating a new queue](images/create-queue-button.png)

In the pop-up window create a new local queue named `MYQUEUE`.

![Create a new queue](images/create-queue.png)

A new queue will be created, take a look at the queue depth, it should be 0.

![Queue depth should be 0](images/queue-depth.png)

To connect to the queue we'll need information such as the hostname and port of the queue manager. To get this information go to the (☰) menu and choose *MQ* > *mq-1* > *Release details*.

![MQ release details](images/mq-release-details.png)

Scroll down to the bottom of page, make a note of the queue manager hostname and listener port.

![MQ connection info](images/mq-connection-info.png)

**MORE TO COME**
