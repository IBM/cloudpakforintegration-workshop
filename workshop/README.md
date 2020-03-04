# Enterprise-grade integration within a micro-service based application on OpenShift, with Cloud Pak for Integration

Welcome to our workshop! In this workshop we'll be using the Cloud Pak for Integration platform to modify an existing micro-service based application to more robustly wire the micro-services together for running in an Enterprise environment on OpenShift, both for Day 0 and beyond. The goals of this workshop are:

* Deploy an API Gateway using `API Connect`, to provide an API management point across the micro-services
* Simplify data access from external data sources, such as Salesforce, using `App Connect`
* Integrate the payment flow into an existing `MQ-based` payment service
* Simplify the flow of status udpates to the end user with a kafka event stream, using `Event Streams`
* Find performace bottlenecks in the overall flow, using `Tracing`

## About this workshop

The introductory page of the workshop is broken down into the following sections:

* [Agenda](#agenda)
* [Compatability](#compatability)
* [About Cloud Pak for Integration](#about-cloud-pak-for-integration)
* [Credits](#credits)

## Agenda

### Day 1: API and Data connectivity and management with Cloud Pak for Integeration

In this first day, as well as introducing the capabilities of Cloud Pak for Integration, we'll learn how to use the API Connect and App Connect components within a micro-services based application on OpenShift.

|   |   |
| - | - |
| [Lecture 1: What is Cloud Native?](https://ibm.box.com/s/3pvl4jdi3xifs1olzcl9np904zvk5ueo) | Learn about the technologies that underpin Cloud Native applications |
| [Lecture 2: Introduction to Cloud Pak for Integration](TBD) | Learn about the set of solutions that enable you to rapidly integrate Cloud Native applications |
| [Exercise 1: Introduction to the example Bee Travels application](exercise-1/README.md) | Install and run the example micro-services based application, ahead of starting to implement enterprise-grade integration capabilities |
| [Lecture 3: Introduction to API Connect](TBD) | Learn about how API Connect can provided a management point for all the APIs in your application |
| [Exercise 2: Add an API Management point to the application](exercise-2/README.md) | Introduce a common management point for all the APIs, with routing and logging for all calls to the Bee Travels application|
| [Lecture 4: Introduction to App Connect](TBD) | Learn about how App Connect can easy the connection of data sources (wherever they live) to your application |
| [Exercise 3: Use App Connect to easily integrate an external data source](exercise-3/README.md) | Integrate Salesforce CRM data into the Bee Travels application |

### Day 2: Adding Events, as well as Tracing to the application

In the second day we'll learn about how to add different classes of events to the application, as well as adding traching capabilities so we can keep track of what is going on.

|   |   |
| - | - |
| [Lecture 5: Introduction to MQ](TBD) | Learn all about the types of events most suitable for MQ, and how to go about integrating that into your application |
| [Exercise 4: Add a payment flow from your application into MQ](exercise-4/README.md) | Integrate an existing MQ payment queue into the Bee Travels application |
| [Lecture 6: Introduction to Event Streams](TBD) | Learn all about the types of events most suitable for Event Streams & Kafka, and how to go about integrating that into your application |
| [Exercise 5: Add a customer notification event process into your application using Event Streams](exercise-5/README.md) | Add a Kafka event stream into the Bee Travels application |
| [Lecture 7: Introduction to Tracing with Cloud Pak for Integration](TBD) | Learn how to build distributed tracing into your application |
| [Exercise 6: Add Tracing into your application](exercise-6/README.md) | Add distributed tracing into the Bee Travels application |

## Compatability

This workshop has been tested on the following platforms:

* **macOS**: Mojave (10.14), Catalina (10.15)
* **Windows** Windows 10 (with enterprise AAD and git bash)

## About Cloud Pak for Integration

IBM Cloud Pak for Integration is an enterprise-ready, containerized software solution that contains all the tools you need to integrqte and connect appication components and data both within and between clouds. Running on Red Hat® OpenShift®, the IBM Cloud Pak for Integration gives businesses complete choice and agility to deploy workloads on premises and on private and public cloud.

Cloud Pak for Integration includes components to enable you to manage:

|   |   |
| - | - |
| API lifecycle | Create, secure, manage, share and monetize APIs across clouds while you maintain continuous availability. Take control of your API ecosystem and drive digital business with a robust API strategy that can meet the changing needs of your users |
| Application and data integration | Integrate all of your business data and applications more quickly and easily across any cloud, from the simplest SaaS application to the most complex systems — without worrying about mismatched sources, formats or standards |
| Enterprise messaging | Simplify, accelerate and facilitate the reliable exchange of data with a flexible and security-rich messaging solution that’s trusted by some of the world’s most successful enterprises. Ensure you receive the information you need, when you need it — and receive it only once |
| Event streaming | Use Apache Kafka to deliver messages more easily and reliably and to react to events in real time. Provide more personalized customer experiences by responding to events before the moment passes |
| High-speed data transfer | Send large files and data sets virtually anywhere, reliably and at maximum speed. Accelerate collaboration and meet the demands of complex global teams, without compromising performance or security |
| Secure gateway | Create persistent, security-rich connections between your on-premises and cloud environments. Quickly set up and manage gateways, control access on a per-resource basis, configure TLS encryption and mutual authentication, and monitor all of your traffic |

![Cloud Pak for Integration Stack](.gitbook/images/cp4apps.png)

## Credits

This workshop was primarily written by [Henry Nash](https://github.com/henrynash) and [Steve Martinelli](https://github.com/stevemar). Many other IBMers have contributed to help shape, test, and contribute to the workshop.

* [A N Other](https://github.com/GregDritschler): For his [Funky Shirts](https://github.com/IBM/appsody-sample-quote-app)
