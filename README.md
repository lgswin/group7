# Observable Monolith - WordPress & Signoz Monitoring System 
## **[Group7] Gunsu Lee, Priya Singh, Mayank Sharma, Akash Dewayalage**

This project is a **containerized web application** powered by **WordPress**, along with **Signoz** for monitoring and observability. The system is designed to track application performance, logs, and metrics in real-time.

## **System Overview**

The system consists of **two primary components**:

1. **Web Application (WordPress)**
    - A WordPress-based CMS running inside a Docker container.
    - Exposes port `8000` for user access.
    - Stores posts and media files in the `wp-content` folder, which is shared with the container.
2. **Monitoring System (Signoz)**
    - Signoz is used for monitoring logs, traces, and performance metrics.
    - Collects observability data through OpenTelemetry.
    - Uses a **ClickHouse** backend for storage.

## **Containers Running in the System**

The following containers are running as part of this system:

| **Container Name** | **Image** | **Purpose** | **Ports** |
| --- | --- | --- | --- |
| **WordPress App** | `observable-monolith-group7-wordpress` | WordPress application container | `0.0.0.0:8000->8000/tcp` |
| **Signoz Frontend** | `signoz/frontend:0.56.0` | Web interface for Signoz monitoring | `0.0.0.0:3301->3301/tcp` |
| **Signoz OpenTelemetry Collector** | `signoz/signoz-otel-collector:0.102.12` | Collects telemetry data (traces, metrics, logs) | `0.0.0.0:4317-4318/tcp` |
| **Signoz Query Service** | `signoz/query-service:0.56.0` | Processes and queries observability data | `8080/tcp` |
| **Signoz Alert Manager** | `signoz/alertmanager:0.23.7` | Sends alerts based on predefined thresholds | `9093/tcp` |
| **Signoz Logspout** | `gliderlabs/logspout:v3.2.14` | Forwards container logs to Signoz | `80/tcp` |
| **ClickHouse DB (Signoz Storage)** | `clickhouse/clickhouse-server:24.1.2-alpine` | Stores logs, metrics, and traces | `0.0.0.0:8123, 9000, 9181/tcp` |
| **Zookeeper (Signoz Support Service)** | `bitnami/zookeeper:3.7.1` | Manages distributed coordination | `0.0.0.0:2181, 2888, 3888/tcp` |

## **How to Start the System**

To start all containers, run: `docker compose up -d`

- This will bring up **WordPress** on port **8000**.
- Signoz UI will be available on **port 3301**.

## **Local WordPress Installation & Deployment**

### **WordPress Content Management**

- When **WordPress is installed and posts are created**, all data is stored inside the `wp-content` folder.

### **Automated Deployment to GitHub Pages**

1. When a new post is added, WordPress updates the `.ht.sqlite` file in `wp-content`.
2. If the changes are committed and pushed to the repository, **GitHub Actions** automatically deploys the WordPress page to **GitHub Pages**.
3. The deployment process is managed using the following GitHub Actions workflow:
    
    ```yaml
    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Run npm install, npm run build and publish to gh-pages
      - uses: diy-pwa/npm-build-2-gh-pages@v1.11
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
    ```
    
4. The **updated WordPress content** will then be deployed to **GitHub Pages** from the `gh-pages` branch.

### **GitHub Pages Deployment**

You can access the deployed WordPress site at:

➡️ https://lgswin.github.io/group7-monolith/


## **Monitoring with Signoz**

1. Open the Signoz UI at `http://localhost:3301`
2. Monitor logs, traces, and metrics in real-time.
3. Use OpenTelemetry to instrument additional services.



## **Completed Tasks**

The following tasks were completed as part of the grading requirements:

1. **Get WordPress Up and Running from the `wp-content` Folder**
    - The repository was cloned locally, and the container was started to install WordPress.
    - Posts created in WordPress were stored in the `wp-content` folder, which is shared between the container and the local system.
2. **Agree on an Idea for the Site and Each Team Member Add One Post**
    - Each team member created or edited their own post and uploaded it to the repository.
3. **Deploy on GitHub Pages**
    - Any new changes pushed to the repository trigger **GitHub Workflows**, which **automatically deploys** the updated content to **GitHub Pages**.
4. **Change the `OTEL_SERVICE_NAME` as in the Requirement**
    - The `OTEL_SERVICE_NAME` was updated in `wordpress/docker-compose.yml`:
        
        ```yaml
        - OTEL_SERVICE_NAME=Group7-Monolithic
        ```
        
5. **Identify a Metric for the Total Compute Time of the WordPress Container**
    - Logged into **Signoz** and created a **dashboard**.
    - Added a **new panel** and configured the **metrics field** to track `system_cpu_time`.
    - This allows monitoring of **compute resource usage** for the WordPress container.
![image](https://github.com/user-attachments/assets/f99bc244-0a83-4bc6-b6b9-c6f56d19cf3b)
