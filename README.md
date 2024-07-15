# Hamada-FYP
## Full SIEM in one Docker Compose File 

![image](https://github.com/user-attachments/assets/049ec463-3dfa-4bdf-b8ae-7ec77e7fb467)


This project is a comprehensive Docker-based setup for a variety of services including Wazuh (a security information and event management system), Traefik (a modern HTTP reverse proxy and load balancer), Grafana, InfluxDB, Telegraf, and more.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Services](#services)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

Before you begin, ensure you have the following installed on your system:

- Docker
- Docker Compose
- Git (for cloning the repository)

---

## Getting Started

1. Clone the repository:
   ```
   git clone https://github.com/Hamada-khairi/Hamada-FYP.git
   cd Hamada-FYP-Simple
   ```

2. Set up the environment variables:
   ```
   cp .env.example .env
   ```
   Edit the `.env` file with your desired configurations.

3. Increase the max map count (required for Wazuh):
   ```
   sudo sysctl -w vm.max_map_count=262144
   ```

4. Generate certificates for the Wazuh indexer:
   ```
   docker-compose -f generate-indexer-certs.yml run --rm generator
   ```
   
5. Generate wild card certificates for Traefik you can change the Domain

```
 openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
     -keyout ./traefik/config/certs/cert-key.pem \
     -out ./traefik/config/certs/cert.pem \
     -subj "/CN=*.hamada.local" \
     -addext "subjectAltName=DNS:*.hamada.local,DNS:hamada.local" 
```

Verifying the Certs
```
 openssl x509 -outform der -in ./traefik/config/certs/cert.pem -out ./traefik/config/certs/cert.der

 openssl x509 -in ./traefik/config/certs/cert.pem -text -noout

 openssl rsa -noout -modulus -in ./traefik/config/certs/cert-key.pem | openssl md5

 openssl x509 -noout -modulus -in ./traefik/config/certs/cert.pem | openssl md5 
```

6. Start the services:
   ```
   docker-compose up -d
   ```

6. Access the services through their respective URLs (as defined in your Traefik configuration).

---


## Project Structure

```
Hamada-FYP-Simple/
├── bind9/
│   ├── config/
│   ├── cache/
│   └── records/
├── homarr/
│   ├── configs/
│   └── icons/
├── influxdb/
│   └── influxdb_data/
├── portainer/
│   └── portainer_data/
├── telegraf/
│   └── telegraf.conf
├── traefik/
│   ├── config/
│   │   ├── conf/
│   │   ├── certs/
│   │   └── traefik.yaml
├── vol/
│   ├── grafana/
│   │   └── grafana_data/
│   └── wazuh/
│       ├── config/
│       └── logs/
├── wazuh/
│   ├── config/
│   │   ├── wazuh_cluster/
│   │   ├── wazuh_indexer/
│   │   ├── wazuh_dashboard/
│   │   └── wazuh_indexer_ssl_certs/
│   └── logo/
├── docker-compose.yml
├── generate-indexer-certs.yml
└── .env
```

---

## Services

This project includes the following services:

1. Traefik: Reverse proxy and load balancer
2. Bind9: DNS server
3. Wazuh Manager: Security information and event management
4. Wazuh Indexer: Data indexing for Wazuh
5. Wazuh Dashboard: Web interface for Wazuh
6. Telegraf: Server agent for collecting and reporting metrics
7. InfluxDB: Time series database
8. Grafana: Analytics and interactive visualization web application
9. Filebrowser: Web-based file manager
10. IT Tools: Collection of handy IT tools
11. Homarr: Dashboard for your server
12. Portainer CE: Container management platform

---

## Configuration

### Traefik

Traefik is configured to handle routing and SSL termination for all services. The configuration can be found in `traefik/config/traefik.yaml`.

### Wazuh

Wazuh configuration files are located in the `wazuh/config/` directory. You may need to adjust these based on your specific security requirements.

### Telegraf, InfluxDB, and Grafana

These services work together to provide monitoring and visualization capabilities. Telegraf configuration can be found in `telegraf/telegraf.conf`.

### Other Services

Configurations for other services can be found in their respective directories or within the `docker-compose.yml` file.

## Troubleshooting

If you encounter any issues:

1. Check the logs of the specific service:
   ```
   docker-compose logs [service_name]
   ```
2. Ensure all required ports are open and not in use by other services.
3. Verify that all environment variables in the `.env` file are correctly set.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

## Security Features

This project incorporates several security measures to protect your infrastructure and data:

1. **Wazuh Security Information and Event Management (SIEM)**
   - Wazuh provides comprehensive security monitoring and threat detection.
   - It offers real-time analysis of security events across your infrastructure.
   - Features include log analysis, file integrity monitoring, vulnerability detection, and compliance monitoring.

2. **Traefik as a Reverse Proxy with SSL/TLS Encryption**
   - Traefik acts as a secure gateway to your services, managing routing and load balancing.
   - It automatically handles SSL/TLS certificate management for HTTPS connections.
   - This ensures encrypted communication between clients and your services, protecting data in transit.

3. **Network Segmentation with Docker Networks**
   - The project uses a custom Docker network (`hamada-network`) to isolate containers.
   - This network segmentation limits the attack surface and contains potential security breaches.
   - It allows for fine-grained control over inter-container communication.

### Additional Security 

- **Bind9 DNS Server**: Provides a local DNS resolution, reducing reliance on external DNS services and potential DNS-based attacks.


---


## License

MIT License

Copyright (c) [2024] [MOHAMED KHAIRY]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
