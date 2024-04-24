# Data Validation with Docker Compose

This project uses Docker Compose to orchestrate multiple containers that provide a robust environment for data validation and visualization. Below is an overview of the services configured in the Docker Compose file and instructions on how to set them up.

## Understanding Containerization

### What is a Container?
A container packages code along with all its dependencies, ensuring that the application runs consistently across different computing environments.

### What is Docker?
Docker is an open-source platform that simplifies the development, shipping, and running of applications by using containerization technology.

### What is Docker Desktop?
Docker Desktop is an Integrated Development Environment (IDE) for managing Docker containers. It provides a graphical interface for easy container management on both Mac and Windows.


## Services Configured

### Grafana
Grafana is an open-source platform for monitoring and observability. It allows you to query, visualize, alert on, and understand your metrics no matter where they are stored. In this setup, Grafana is pre-configured with the `grafana-worldmap-panel` plugin.
- **Port:** 3000

- **Version:** No specific version tag is specified. It defaults to the `latest` version.
- **GitHub:** [Grafana GitHub](https://github.com/grafana/grafana)
- **Docker Hub:** [Grafana Docker Hub](https://hub.docker.com/r/grafana/grafana)

### Mosquitto
Eclipse Mosquitto is an open source message broker that implements the MQTT protocol. It's lightweight and suitable for use on all devices from low power single board computers to full servers.
- **Ports:**
  - MQTT: 1883
  - Websockets: 80 (if configured)
- **Version:** No specific version tag is specified. It defaults to the `latest` version.
- **GitHub:** [Eclipse Mosquitto GitHub](https://github.com/eclipse/mosquitto)
- **Docker Hub:** [Mosquitto Docker Hub](https://hub.docker.com/_/eclipse-mosquitto)

### Node-RED
Node-RED is a programming tool for wiring together hardware devices, APIs, and online services in new and interesting ways. It provides a browser-based editor that makes it easy to wire together flows using the wide range of nodes in the palette that can be deployed to its runtime in a single click.
- **Port:** 1880
- **Version:** No specific version tag is specified. It defaults to the `latest` version.
- **GitHub:** [Node-RED GitHub](https://github.com/node-red/node-red)
- **Docker Hub:** [Node-RED Docker Hub](https://hub.docker.com/r/nodered/node-red)

### pgAdmin
pgAdmin is a web-based administration tool for PostgreSQL. It allows you to manage PostgreSQL 9.2 and above from the web.
- **Port:** 5050
- **Version:** No specific version tag is specified. It defaults to the `latest` version.
- **GitHub:** [pgAdmin GitHub](https://github.com/postgres/pgadmin4)
- **Docker Hub:** [pgAdmin Docker Hub](https://hub.docker.com/r/dpage/pgadmin4)

### TimescaleDB
TimescaleDB is an open-source database designed to make SQL scalable for time-series data. It is packaged as a PostgreSQL extension and thus fully compatible with PostgreSQL and the rich ecosystem of Postgres tools.
- **Port:** 5432
- **Version:** `pg16-all-amd64` - This tag specifies the PostgreSQL 16 compatible version of TimescaleDB with a high availability configuration.
- **GitHub:** [TimescaleDB GitHub](https://github.com/timescale/timescaledb)
- **Docker Hub:** [TimescaleDB Docker Hub](https://hub.docker.com/r/timescale/timescaledb-ha), exact version: `timescale/timescaledb-ha:pg16-all-amd64`

## Customizing Versions
For this project, we have chosen to work with the latest version of each container to take advantage of the most recent features and improvements. Docker Compose allows us to specify which version of each container is used by modifying the `image` field in the `docker-compose.yml` file. By default, if no tag is specified after the image name, Docker uses the `latest` tag, pulling the most recent version available on Docker Hub.

It is possible to choose what version is running by specifying a version tag in the `image` field of the Docker Compose file. For example, if you want to use a specific version of Grafana, you can modify the image line like this:

```yaml
services:
  grafana:
    image: grafana/grafana:8.3.0  # Example version tag
```

This modification will ensure that Docker pulls the specified version of the Grafana image instead of the latest. Specifying exact version tags is recommended to maintain consistency across different environments and to prevent unexpected behavior changes due to updates in new versions.


## Prerequisites

### Installing Docker Desktop
Before setting up the project, you need to install Docker Desktop:
- **For Windows and Mac:** Visit [Docker Desktop](https://www.docker.com/products/docker-desktop) and download the installer for your operating system. Follow the installation instructions provided.
- **For Linux:** Docker Desktop is not available, but you can install Docker Engine directly from your distribution's repository. Follow the instructions on the [Docker website](https://docs.docker.com/engine/install/).

## Installation Guide

1. **Clone the repository:**
   ```bash
   git clone https://github.com/IdemoLab/DataValidation
   cd DataValidation
   ```

2. **Adapt the environment file:**
   The project contains a `.env` file. Adapt the variables in this file to suit your configuration before starting the services.

3. **Start the services:**
   ```bash
   docker-compose up -d
   ```
   This command will download the images, create the containers, and start the services.

4. **Access the services:**
   Open a browser and type `localhost:{port}` where `{port}` corresponds to the services:
   - Node-RED: 1880
   - pgAdmin: 5050
   - Grafana: 3000

## Additional Configuration

### Node-RED
- Optionally, set up a password for additional security.
- Install specific nodes as needed and import flows to enhance functionality.

### pgAdmin
- Connect to your PostgreSQL databases such as TimescaleDB:
  1. Open PGAdmin and log in.
  2. Click “Add New Server” and configure connection settings using credentials defined in your `.env` file.

### Grafana
- To connect to PostgreSQL in Grafana:
  1. Navigate to Connections → Data Sources → New data source → PostgreSQL.
  2. Fill in the details like Name, Connection, and Authentication using information from your `.env` file.


## Managing the Services

To stop all services, you can use:
```bash
docker-compose down
```

To view logs for a specific service:
```bash
docker-compose logs -f [service_name]
```

For more detailed operations such as restarting a service, removing volumes, or scaling services, refer to the Docker Compose documentation.