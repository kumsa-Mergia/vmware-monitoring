# VMware Monitoring with Docker Compose

This project provides a simple setup for monitoring VMware vSphere environments using the **vmware_exporter** and Docker Compose. It exposes metrics that can be scraped by Prometheus and visualized in Grafana.

---

## 📌 Overview

This repository runs multiple VMware exporters as Docker containers to monitor different vCenter instances.

Each exporter:

* Connects to a vCenter Server
* Collects metrics (VMs, hosts, datastores, etc.)
* Exposes them via HTTP for Prometheus

---

## 🧱 Architecture

* **vmware_exporter_dc1** → Primary Datacenter
* **vmware_exporter_dc2** → Secondary Datacenter
* Metrics exposed on:

  * `http://localhost:9272/metrics`
  * `http://localhost:9273/metrics`

---

## ⚙️ Requirements

* Docker
* Docker Compose
* Network access to vCenter servers
* Read-only vSphere user accounts (recommended)

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/kumsa-Mergia/vmware-monitoring.git
cd vmware-monitoring
```

---

### 2. Create Environment File

Create a `.env` file in the project root:

```env
DC1_HOST=your-vcenter-1
DC1_USER=your-username
DC1_PASSWORD=your-password

DC2_HOST=your-vcenter-2
DC2_USER=your-username
DC2_PASSWORD=your-password
```

> ⚠️ **Important:** Do NOT commit `.env` to Git.

---

### 3. Start the Services

```bash
docker-compose up -d
```

---

### 4. Verify

Check if exporters are running:

```bash
docker ps
```

Access metrics in your browser:

* [http://localhost:9272/metrics](http://localhost:9272/metrics)
* [http://localhost:9273/metrics](http://localhost:9273/metrics)

---

## 📊 Prometheus Configuration (Example)

Add the following to your Prometheus config:

```yaml
scrape_configs:
  - job_name: 'vmware_dc1'
    static_configs:
      - targets: ['localhost:9272']

  - job_name: 'vmware_dc2'
    static_configs:
      - targets: ['localhost:9273']
```

---

## 📈 Grafana

Import VMware dashboards or create your own using Prometheus as a data source.

---

## 🔒 Security Best Practices

* Never store credentials in `docker-compose.yml`
* Use `.env` files or secret management tools
* Use read-only vSphere accounts
* Rotate passwords regularly
* Add `.env` to `.gitignore`

---

## 🛠️ Troubleshooting

* Ensure DNS servers are reachable from the container
* Verify vCenter hostname/IP is correct
* Check logs:

```bash
docker logs vmware_exporter_dc1
docker logs vmware_exporter_dc2
```

---

## 📂 Project Structure

```
vmware-monitoring/
│── docker-compose.yml
│── .env (not committed)
│── README.md
```

---

## 🤝 Contributing

Contributions are welcome. Feel free to open issues or submit pull requests.

---

---