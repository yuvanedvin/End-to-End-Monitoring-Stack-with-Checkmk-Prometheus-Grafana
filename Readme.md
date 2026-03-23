📊 Monitoring Stack with Checkmk, Prometheus & Grafana (Docker Setup)
🧭 Overview
This project demonstrates a practical monitoring setup using:
•	Checkmk for infrastructure monitoring and alerting
•	Prometheus for metrics collection and storage
•	Grafana for visualization and dashboards
All components are containerized using Docker for easy setup and portability.
The goal of this project was to build a monitoring stack that separates responsibilities cleanly while still working together as a cohesive system.

🏗️ Architecture
[ Hosts / Services ]
        ↓
     Checkmk
        ↓ (Prometheus endpoint)
     Prometheus
        ↓ (PromQL queries)
      Grafana

•	Checkmk collects and evaluates metrics
•	Prometheus scrapes metrics from Checkmk
•	Grafana queries Prometheus and displays dashboards

🚀 Tech Stack
•	Docker & Docker Compose
•	Checkmk (Raw Edition)
•	Prometheus
•	Grafana

⚙️ Setup Instructions
1. Clone the repository
git clone <your-repo-url>
cd monitoring-stack


2. Start services
docker compose up -d


3. Access the services
Tool	URL
Checkmk	http://localhost:8080/mysite

Prometheus	http://localhost:9090

Grafana	http://localhost:3000


4. Default credentials
Checkmk
•	Username: cmkadmin
•	Password: cmkadmin
Grafana
•	Username: admin
•	Password: admin

🔧 Configuration Steps
1. Enable Prometheus endpoint in Checkmk
•	Navigate to: Setup → Integrations → Prometheus
•	Enable metrics endpoint

2. Create automation user in Checkmk
•	Go to: Setup → Users
•	Create user with:
o	Authentication: Automation secret
o	Role: Administrator
This user is used by Prometheus to authenticate while scraping metrics.

3. Configure Prometheus
Update prometheus.yml:
scrape_configs:
  - job_name: 'checkmk'
    metrics_path: /mysite/prometheus
    static_configs:
      - targets: ['checkmk:5000']
    basic_auth:
      username: prometheus
      password: <automation_secret>

Restart Prometheus after changes.

4. Add Prometheus in Grafana
•	Go to: Connections → Data Sources
•	Add Prometheus
•	URL:
http://prometheus:9090


📊 Using the Stack
Monitoring (Checkmk)
Checkmk is the main tool for:
•	Viewing host/service status
•	Receiving alerts
•	Investigating failures

Metrics & Queries (Grafana)
Grafana is used for:
•	Exploring metrics
•	Building dashboards
•	Analyzing trends
Example queries:
up{job="checkmk"}
checkmk_service_state


Backend (Prometheus)
Prometheus runs in the background:
•	Scrapes metrics from Checkmk
•	Stores time-series data
•	Executes queries from Grafana

🧪 Observations & Learnings
•	Checkmk requires proper service discovery before metrics appear
•	Prometheus uses a pull model, so endpoint configuration is critical
•	Authentication (automation secret) is mandatory for scraping
•	Metric names are not always obvious — exploration is necessary

⚖️ Pros & Cons
✅ What worked well
•	Clear separation of concerns
•	Strong alerting via Checkmk
•	Flexible visualization with Grafana
•	Easy deployment using Docker

❌ Challenges faced
•	Initial setup required careful configuration
•	Metric discovery took some trial and error
•	Managing three tools adds complexity

📌 Use Cases
•	Infrastructure monitoring
•	System performance tracking
•	Capacity planning
•	Debugging production issues

🧠 Key Takeaway
Instead of relying on a single tool, this setup combines:
•	Monitoring (Checkmk)
•	Storage (Prometheus)
•	Visualization (Grafana)
This makes the system more flexible and closer to real-world production setups.

🔮 Future Improvements
•	Add alert integration (Slack/Email)
•	Secure endpoints with HTTPS
•	Add more exporters (node-exporter, app metrics)
•	Create reusable Grafana dashboards

📷 Screenshots
(Add your Grafana and Checkmk screenshots here)

🙌 Final Notes
This project helped me understand how modern monitoring stacks are built and how different tools interact with each other. It also gave hands-on experience with PromQL, Docker networking, and monitoring concepts.


