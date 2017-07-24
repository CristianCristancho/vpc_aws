# SQS, SWF, and SNS

## simple Queue Service (SQS)
Message queuing service to decouple the components of a cloud application. you can store application messages on reliable and scalable infrastructure,
enabling you to move data between distributed components to perform different tasks as needed.

Is basically a buffer between the application components that receive data and those components that process the data in your system. If your processing servers cannot process the work fast enough (perhaps due to a spike in traffic), the work is queued so that the processing servers can get to it when they are ready. This means that work is not lost due to insufficient resources.

---

SQS ensures delivery of each message at least once and supports multiple readers and writers interacting with the same queue. A single queue can be used simultaneously by many distributed application components, with no need for those components to coordinate with one another to share the queue. Although most of the time each message will be delivered to your application exactly once, you should design your system to be idempotent.

The service does not guarantee FIFO delivery of messages. If your system requires that order be preserved, you can place sequencing information in each message so that you can reorder the messages when they are retrieved from the queue.

---

### Message lifecycle

 ![lifecicle](figures\chapter8-figure1-MessageLifecicle.PNG "describes the lifecycle of an Amazon SQS message, called Message A, from creation to deletion. Assume that a queue already exists.")

---

### Queue and Message Identifiers
Messages are identified via a globally unique ID that Amazon SQS returns when the message is delivered to the queue.
SQS uses three identifiers that you need to be familiar with: queue URLs, message IDs, and receipt handles.

### Message Attributes
Provides support for message attributes. Message attributes allow you to provide structured metadata items (such as timestamps, geospatial data, signatures, and identifiers) about the message.

---

### Dead Letter Queues
Is a queue that other (source) queues can target to send messages that for some reason could not be successfully processed.

## Amazon Simple Workflow Service (Amazon SWF)
Makes it easy to build applications that coordinate work across distributed components. SWF gives you full control over implementing and coordinating tasks without worrying about underlying complexities such as tracking their progress and
maintaining their state.

---

SWF implement workers to perform tasks. Workers can run either on cloud infrastructure, such as Amazon EC2, or on your own premises. SWF stores tasks, assigns them to workers when they are ready, monitors their progress, and maintains their state, including details on their completion. SWF maintains an application’s execution state durably so that the application is resilient to failures in individual components.

---

### Workflows
Coordinate and manage the execution of activities that can be run asynchronously across multiple computing devices.

#### Workflow Domain
Provide a way of scoping Amazon SWF resources within your AWS account. You must specify a domain for all the components of a workflow, such as the workflow type and activity types. It is possible to have more than one workflow in a domain; however, workflows in different domains cannot interact with one another.

#### Workflow History
Is a detailed, complete, and consistent record of every event that occurred since the workflow execution started.

---

### Actors
Actors can be workflow **starters**, **deciders**, or activity **workers**. These actors communicate with Amazon SWF through its API. You can develop actors in any programming language.

**Starter** is any application that can initiate workflow executions.

Activities within a workflow can run sequentially, in parallel, synchronously, or asynchronously. The logic that coordinates the tasks in a workflow is called the **decider.** The decider schedules the activity tasks and provides input data to the activity workers.

An activity **worker** is a single computer process that performs the activity tasks in your workflow.

---

### Tasks
We have three types of tasks: activity tasks, AWS Lambda tasks, and decision tasks.
  - An activity task tells an activity worker to perform its function, such as to check inventory or charge a credit card.
  - AWS Lambda task is similar to an activity task, but executes an AWS Lambda function instead of a traditional Amazon SWF activity.
  - A decision task tells a decider that the state of the workflow execution has changed so that the decider can determine the next activity that needs to be performed.

---

### Task lists
Provide a way of organizing the various tasks associated with a workflow. You could think of task lists as similar to dynamic queues. Provide a flexible mechanism to route tasks to workers as your use case necessitates.

---

## Amazon Simple Notification Service (Amazon SNS)
Web service for mobile and enterprise messaging that enables you to set up, operate, and send notifications. SNS follows the publish-subscribe (pub-sub) messaging paradigm, with notifications being delivered to clients using a push mechanism that eliminates the need to check periodically (or poll) for new information and updates.

SNS consists of two types of clients: **publishers** and **subscribers** (sometimes known as producers and consumers). Publishers communicate to subscribers asynchronously by sending a message to a topic. A **topic** is simply a logical access point/communication channel that contains a list of subscribers and the methods used to communicate to them

---

 ![messages](figures\chapter8-figure2-SNS_TopicDelivery.PNG "shows this process at a high level. A publisher issues a message on a topic. The message is then delivered to the subscribers of that topic using different methods, such as Amazon SQS, HTTP, HTTPS, email, SMS, and AWS Lambda.")
