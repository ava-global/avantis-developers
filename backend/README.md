# 🚀 Backend

![](Backend/assets/avantis-techstack\_Backend-Infra.png)

## **Infrastructure**

* **AWS** - We use AWS as our cloud infrastructure provider with components below
  * **Data Pipeline** - Our data pipeline consist of
    * **RDBMS** - We use **AWS Aurora PostgreSQL** as our relational database management system. The benefit of AWS Aurora is that it has high reliability, availability, and scalability which meet our requirements to grow the infrastructure together with our business.
    * **Cache** - We use **Redis** as a cache layer to reduce the number of requests that need to access our RDBMS directly. This will improve our performance by reducing the response time and also reducing the load of RDBMS.
    * **Event Sourcing -** We use Kaffa, AWS MSK, as our event sourcing to transport the data from source to destination e.g. Data from the data lake to data warehouse, Log & Metric from applications to storage.
  * **Kubernetes (EKS) - We use Kubernetes to run our microservices with 2 layers auto-scaling which are**
    * **Microservice Auto Scaling** - When the specific microservice load reaches the configured threshold, it will automatically horizontal scale the microservice in response to the high load at that time.
    * **Kubernetes Node Autoscaling** - When a load of Kubernetes nodes reaches the configured threshold, it will automatically horizontally scale the node to support more microservices.
  * **API Gateway -** We use AWS API Gateway to handle all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, CORS support, authorization, access control, throttling, monitoring.
* **Infrastructure As A Code**
  * With all the infrastructure mentioned above, we build them using **Pulumi** to define our cloud infrastructure resources. The benefit of using Pulumi is that everything is defined in the programming language which means that we can add unit testing over it and when something goes wrong, we can fix or recreate the same cloud infrastructure within less than an hour by just running the code.

## **Application Development**

* **Core Backend Microservices**
  * **Rust** - We use **Rust** as our main development programming language for all of our microservices. The benefit of Rust is that
    * Rust's Static Typing Ensures Easy Maintainability.
    * Rust Has Fast and High Performance with low memory consumption.
    * As Fast As C-programming language without Memory Management Problems.
    * Rust encourages engineers to write good code.
    * etc.
  * **GraphQL** - All of our API is developed with GraphQL standard. The benefit of GraphQL is that it has No over-fetching and under-fetching problems. Consumers of the API can choose by themselves which information they need.
  * **gRPC** - All internal microservice to microservice communication are executed on gRPC protocol which offers higher performance advantages through the Protobuf message structure compared to the traditional REST API.
* **BFF ( Backend for Frontend ) -** BFF is the center of Backend-to-Frontend data transformation. We develop all data transformation logic here to reuse this logic for all the frontend platforms i.e. IOS, Android, Web. We develop it with **Kotlin** and **Spring Boot Framework**.
* **Fully Automated Quality Assurance**
  * Unit test with more than 90% of code coverage.
  * Automate Integration Test.
  * Automate End-to-End Test.
  * Automate Source Code and Security auditing.
* **Open Telemetry** - All of our microservices are integrated with Log, Metric, and Tracing systems using Open Telemetry standard.

## **System Monitoring**

* **Metric** - We use **Prometheus** as our metrics storage and all metrics can be used to build a Monitoring Dashboard by **Grafana**.
* **Log** - We store all of our logs data in **ElasticSearch** and all logs from all microservices are viewable via **Kibana.**
* **Tracing** - We use **Jaeger** as end-to-end distributed tracing. With Jager, we can trace a request since it reaches our BFF and sees all the interactions between all related microservices.

## **Service Mesh**

We use **Linkerd** to handle communication between all microservices in our system. It uses Highly intelligent load-balancing algorithms to distribute traffic between each microservices. Allows request routing dynamically and shifts traffic accordingly with features such as resilience, observability, and load balancing.

## **Continuous Integration and Delivery**

However, with all the technologies mentioned above, it will be meaningless if we cannot deliver our microservices to Production as fast as possible. That is why we pay attention to our CI/CD system which are

* **Github and Github Action -** For source code maintenance, unit testing, integration testing, end-to-end testing, and final deployment package building. After the final deployment package building is done, it will be uploaded to **AWS Elastic Container Registry** to be ready for deployment.
* **Argo CD -** We use Argo CD as our continuous delivery system. After the latest deployment package is done and uploaded to Container Registry, Argo CD will run and automatically fetch the latest version of the deployment package and then deploy it to our Kubernetes System within minutes.
