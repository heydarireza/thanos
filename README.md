# Docker Compose for Thanos Stack
 [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

This readme file provides instructions on how to create a Docker Compose for Thanos stack, a highly available Prometheus setup that enables long-term storage and global querying capabilities.

## Prerequisites

Before proceeding, ensure you have the following installed on your system:

1. Docker: Make sure you have Docker installed and running on your machine.
2. Docker Compose: Install Docker Compose, which comes bundled with Docker Desktop on some platforms.

## Getting Started

To create a Docker Compose stack for Thanos, follow the steps below:

1. **Download the necessary configuration files:**

```bash 
git clone https://github.com/heydarireza/thanos.git
```
2. **Update Configuration:**

   Go through the configuration file (`.env`)  and customize them according to your requirements. You may need to set appropriate storage configurations and username and password.

3. **Docker compose File**


*   thanos component
      *  Sidecar: connects to Prometheus, reads its data for query and/or uploads it to cloud storage.
      *  Compactor: compacts, downsamples and applies retention on the data stored in the cloud storage bucket.
      *  Receiver: receives data from Prometheus’s remote write write-ahead log, exposes it, and/or uploads it to cloud storage.
      *  Ruler/Rule: evaluates recording and alerting rules against data in Thanos for exposition and/or upload.
      * Querier/Query: implements Prometheus’s v1 API to aggregate data from the underlying components.
      * Query Frontend: implements Prometheus’s v1 API to proxy it to Querier while caching the response and optionally splitting it by queries per day.
![Thanos overview](thanos/Thanos%20High%20Level%20Arch%20Diagram%20with%20Recieve.png )

*   prometheus 
Prometheus is an open-source monitoring and alerting toolkit originally developed by SoundCloud. It is designed to collect and store time-series data from various systems and applications, allowing users to monitor their infrastructure's performance, health, and other metrics in real-time.

Key features of Prometheus include:

1. **Time-Series Data Collection:** Prometheus scrapes and collects time-series data from targets, which can be applications, services, or infrastructure components exposing metrics in a specific format known as the Prometheus exposition format.

2. **Data Storage and Retention:** Collected data is stored in a time-series database optimized for efficient querying and retrieval. Prometheus uses its custom storage format to store data with configurable retention periods.

3. **Powerful Query Language:** Prometheus Query Language (PromQL) enables users to perform flexible and expressive queries to retrieve and analyze time-series data. This allows for the creation of custom graphs and visualizations in Grafana or other visualization tools.

4. **Service Discovery:** Prometheus supports various service discovery mechanisms, including static configuration files, Kubernetes, Consul, and others, making it easy to adapt to dynamic environments.

5. **Alerting and Alertmanager Integration:** Prometheus allows users to define alerting rules based on specific conditions. When an alert is triggered, it can be sent to Alertmanager, which then handles alert routing and notifications.

6. **Scalability and Federation:** Prometheus can be deployed in a scalable manner, and it supports federation, allowing multiple Prometheus instances to be connected to form a larger, distributed monitoring system.

7. **Exporters and Integrations:** Prometheus has a rich ecosystem of exporters, which are specialized components that collect metrics from various third-party systems and applications. This makes it easy to integrate with a wide range of services and platforms.

   Prometheus has gained significant popularity due to its simplicity, reliability, and effectiveness in monitoring dynamic cloud-native environments, microservices, and containerized applications. When combined with other tools like Grafana for visualization and Alertmanager for alerting, Prometheus forms a comprehensive monitoring solution used by DevOps teams to ensure the stability and performance of their systems.


*   node exporter
      * Node Exporter is an open-source Prometheus exporter specifically designed to collect and expose various system-level metrics from a Linux or Unix-like operating system. It is commonly used to monitor the health and performance of individual nodes or servers in a distributed system.

      * The Node Exporter runs as a daemon on the target machine and periodically scrapes system-level metrics from the operating system and hardware. It then exposes this collected data in a format that Prometheus can understand. Prometheus, in turn, collects the metrics from the Node Exporter and stores them in its time-series database for further analysis, visualization, and alerting

*   Prometheus rules:
Prometheus rules are a crucial aspect of the Prometheus monitoring system. They define conditions and expressions to evaluate and generate alerts or to create new time-series based on existing metrics. Prometheus uses a rule evaluation engine to process these rules and continuously monitor the defined conditions.

      Prometheus supports two types of rules:
      1. **Recording Rules:**
      2. **Alerting Rules:**
      Alerting rules are used to define conditions that, when met, generate alerts. These rules include an expression that evaluates to a boolean value (1 for true, 0 for false). When the expression evaluates to true, an alert is triggered and sent to the Alertmanager, which handles the alerting process, such as routing and notification.

*   AlertManager
      * Alertmanager is an open-source component of the Prometheus monitoring system, designed to handle and manage alerts generated by Prometheus and other monitoring tools. It acts as a central hub for processing and sending out alerts to various alert receivers, such as email, Slack, PagerDuty, or other communication channels.


---

**Important:**

You have to  update configuration file in [alertmanger config](prometheus/alert/alertmanager.yml)

---

*   grafana



      * Grafana is an open-source data visualization and monitoring tool that allows users to create interactive and customizable dashboards for analyzing and monitoring various metrics from different data sources. It is widely used in the DevOps and IT communities to visualize time-series data and gain insights into the performance and health of systems, applications, and infrastructure.
      * with our config, it can visualize your node-exporter, but you can add another dashboard to it easily. 
      [Grafana Dashboards](https://grafana.com/grafana/dashboards/)
      

*   cadvisor
      * cAdvisor operates as a daemon that runs on each container host and collects various metrics, such as CPU usage, memory consumption, network traffic, and filesystem usage, for each running container on that host. 
      * cAdvisor can be integrated with Prometheus, a popular monitoring and alerting toolkit, to store and visualize performance data for longer periods and set up alerts based on defined thresholds.

*   Remote write
      * Remote Write is a feature in Prometheus that allows the system to send its collected time-series data to a remote storage or monitoring system instead of storing it locally in its built-in time-series database. This feature is useful for offloading the storage and long-term retention of metrics data to a separate, centralized storage solution, which can be more scalable and suited for handling large volumes of data over extended periods.

**Configuration and Integrations:**

You can check [prometheus remote write configuration](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#remote_write) page for more details.

Moreover, we use thanos in this repository, you can also check [this page](https://prometheus.io/docs/operating/integrations/) for more information.

---



The primary purpose of Remote Write is to decouple the storage layer from Prometheus, allowing users to integrate Prometheus with various remote storage backends or monitoring systems. 

4. **Run the Stack:**

   Open a terminal or command prompt and navigate to the directory where the `docker-compose.yaml` file is located.

   Use the following commands to  run the Thanos stack:

   ```bash
   docker-compose up -d
   ```

   The `-d` flag runs the services in the background (detached mode).

5. **Accessing Thanos Components:**

   Depending on the configurations and ports you set in the `.env` file, you can access different Thanos components through their respective endpoints. For example:

   - Thanos Query: `http://localhost:19193`
   - Thanos Store API: `http://localhost:39191`
   - Thanos Compact API: `http://localhost:10902`
   - Thanos Rule: `http://localhost:10932`
   - Grafana Dashboard: `http://localhost:3000`
   - Alertmanager Dashboard: `http://localhost:9093`
   - Minio :  `http://localhost:19001`


   Remember to use the appropriate IP or domain if you are deploying this stack on a remote server.

6. **Shut Down the Stack:**

   To stop and remove the Thanos stack and its containers, run the following command:

   ```bash
   docker-compose down
   ```

## Conclusion

Congratulations! You've successfully set up a Docker Compose stack for Thanos. You can now utilize the power of Thanos to store Prometheus data long-term and perform global queries across multiple Prometheus instances.

Remember to refer to the official Thanos documentation for more advanced configurations and options.

Happy monitoring and querying!