# 🖥️ Sample Homelab Bogdan David

A curated collection of Docker Compose stacks for a self-hosted server environment. This project organizes various services—from home automation and media serving to infrastructure and monitoring—into isolated, manageable Docker Compose configurations.

---

## 🚀 Project Overview

This repository encapsulates the entire server infrastructure, split into distinct, composable stacks. Each folder ending in `Stack` contains its own `docker-compose.yml` file, allowing for independent deployment, updating, and scaling of service groups.

| Stack Name | Directory | Primary Focus                                                                |
| :--- | :--- |:-----------------------------------------------------------------------------|
| **Home Assistant Stack** | `HomeAsisstantStack` | Home automation and connected devices / cameras management (e.g. MotionEye). |
| **Infrastructure Stack** | `InfrastructureStack` | Core services and shared components.                                         |
| **Media Stack** | `MediaStack` | Media consumption and management (e.g., Emby).                               |
| **Monitoring Stack** | `MonitoringStack` | System health, logging, and visualization (Grafana, Prometheus).             |
| **Networking Stack** | `NetworkingStack` | Network utilities and services. (PiHole, VPN)                                |

---

## 🛠️ Installation & Setup

### Prerequisites

Before running any stack, ensure you have **Docker** and **Docker Compose** installed on your host machine.

### Getting Started

1.  **Clone the Repository:** Clone this project locally to your server:
    ```bash
    git clone https://github.com/BVDavid/BDV-DockerFolder-ServerMiniPC
    cd BDV-DockerFolder-ServerMiniPC
    ```

2.  **Configure Environment Variables (Optional but Recommended):**
    For enhanced portability and security, create a `.env` file in this root directory to store sensitive information (like passwords, API keys, host paths for persistent data).

3.  **Deploy a Stack:** Navigate into the desired stack directory and run `docker compose up`.

    **Example: Starting the Media Stack**
    ```bash
    cd MediaStack
    docker compose up -d
    ```

---

## 🧩 Individual Stack Details

### 🏠 Home Assistant Stack

This stack is dedicated to home control and surveillance.

* **MotionEye:** Manages local cameras.
    * **Configuration Location:** Persistence is configured **relative to the `motioneye/docker-compose.yml` file** itself for portability.
    * **Internal Ports:** Exposes the web UI on **`8765`**.

### 📺 Media Stack

Handles media serving and organization.

* **Emby:** Streaming media server. *(Further configuration details would go here based on its `docker-compose.yml`)*

### 📊 Monitoring Stack

Provides observability into the server's health and performance.

* **Grafana & Prometheus:** Used for time-series data collection and visualization.
    * **Access:** Grafana is accessible on port `3000`.

---

## ⚙️ Key Configuration Notes

### Volume Portability

To ensure the project is portable across different host machines:

* **Relative Bind Mounts:** For service-specific data (like in `motioneye`), **relative paths are used** within their respective `docker-compose.yml` files (e.g., `./config/data:/var/lib/motioneye`). This means the data folders are created next to the stack's compose file.
* **Absolute Paths:** System-level mappings (like `/etc/localtime`) remain absolute as they reference host system resources.

### Device Passthrough (MotionEye Specific)

The `motioneye` service explicitly passes video devices to the container:

```yaml
    devices:
      - "/dev/video0:/dev/video0"
      - "/dev/video0:/dev/video1"
