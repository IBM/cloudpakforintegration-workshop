# Enterprise-grade integration within a micro-service based application on OpenShift, with Cloud Pak for Integration

Welcome to our workshop! In this workshop we'll be using the Cloud Pak for Integration platform to modify an existing micro-service based application to more robustly wire the micro-services together for running in an Enterprise environment on OpenShift, both for Day 0 and beyond. The goals of this workshop are:

* Deploy an API Gateway using `API Connect`, to provide an API management point across the micro-services
* Simplify data access from external data sources, such as Salesforce, using `App Connect`
* Integrate the payment flow with `MQ` or `Event Streams`

## About this workshop

The introductory page of the workshop is broken down into the following sections:

* [About Cloud Pak for Integration](#about-cloud-pak-for-integration)
* [Credits](#credits)

## About Cloud Pak for Integration

IBM Cloud Pak for Integration is an enterprise-ready, containerized software solution that contains all the tools you need to integrate and connect appication components and data both within and between clouds. Running on Red Hat OpenShift, the IBM Cloud Pak for Integration gives businesses complete choice and agility to deploy workloads on premises and on private and public cloud.

Cloud Pak for Integration includes components to enable you to manage:

|   |   |
| - | - |
| **API lifecycle** | Create, secure, manage, share and monetize APIs across clouds while you maintain continuous availability. Take control of your API ecosystem and drive digital business with a robust API strategy that can meet the changing needs of your users |
| **Application and data integration** | Integrate all of your business data and applications more quickly and easily across any cloud, from the simplest SaaS application to the most complex systems - without worrying about mismatched sources, formats or standards |
| **Enterprise messaging** | Simplify, accelerate and facilitate the reliable exchange of data with a flexible and security-rich messaging solution that’s trusted by some of the world’s most successful enterprises. Ensure you receive the information you need, when you need it - and receive it only once |
| **Event streaming** | Use Apache Kafka to deliver messages more easily and reliably and to react to events in real time. Provide more personalized customer experiences by responding to events before the moment passes |
| **High-speed data transfer** | Send large files and data sets virtually anywhere, reliably and at maximum speed. Accelerate collaboration and meet the demands of complex global teams, without compromising performance or security |
| **Secure gateway** | Create persistent, security-rich connections between your on-premises and cloud environments. Quickly set up and manage gateways, control access on a per-resource basis, configure TLS encryption and mutual authentication, and monitor all of your traffic |

![Cloud Pak for Integration](.gitbook/images/cp4int.png)

## Credits

This workshop was primarily written by [David Carew](https://github.com/djccarew), [Henry Nash](https://github.com/henrynash) and [Steve Martinelli](https://github.com/stevemar).
