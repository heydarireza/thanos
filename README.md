# Docker Compose Stack for Thanos

This readme file provides instructions on how to create a Docker Compose stack for Thanos, a highly available Prometheus setup that enables long-term storage and global querying capabilities.

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

*   prometheus cluster
*   thanos component
*   AlertManager
*   grafana
*   cadadvisor


4. **Run the Stack:**

   Open a terminal or command prompt and navigate to the directory where the `docker-compose.yaml` file is located.

   Use the following commands to  run the Thanos stack:

   ```bash
   docker-compose up -d
   ```

   The `-d` flag runs the services in the background (detached mode).

5. **Accessing Thanos Components:**

   Depending on the configurations and ports you set in the `thanos.yml` file, you can access different Thanos components through their respective endpoints. For example:

   - Thanos Query: `http://localhost:9090`
   - Thanos Store API: `http://localhost:10901`
   - Thanos Compact API: `http://localhost:10902`

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