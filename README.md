# Wazuh Single-Node Deployment using Docker

This guide explains how to deploy a single-node Wazuh stack using Docker.

## Prerequisites

Ensure the following tools are installed on your system:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- A stable internet connection

## Steps to Install Wazuh Single-Node Deployment

### 1. Clone the Wazuh Docker Repository
Clone the Wazuh Docker repository to your local machine:

```bash
git clone https://github.com/zoecyber001/wazuh-docker-single-node.git 
```

Navigate to the `single-node` directory:

```bash
cd single-node
```

### 2. Generate SSL Certificates
SSL certificates are required for secure communication between nodes.

#### Option A: Generate Self-Signed Certificates
Use Docker Compose to generate the necessary certificates:

```bash
docker-compose -f generate-certs.yml run --rm generator
```

The certificates will be saved in the `config/wazuh_indexer_ssl_certs` directory.

#### Option B: Use Your Own Certificates
Place your existing certificates in the appropriate locations under the `config/wazuh_indexer_ssl_certs` directory. Ensure the file names match the expected names:

- **Wazuh Indexer:**
  - `root-ca.pem`
  - `wazuh.indexer-key.pem`
  - `wazuh.indexer.pem`
  - `admin.pem`
  - `admin-key.pem`

- **Wazuh Manager:**
  - `root-ca-manager.pem`
  - `wazuh.manager.pem`
  - `wazuh.manager-key.pem`

- **Wazuh Dashboard:**
  - `wazuh.dashboard.pem`
  - `wazuh.dashboard-key.pem`
  - `root-ca.pem`

### 3. Update the Docker Compose Configuration
Ensure all services in `docker-compose.yml` are using compatible versions. For example:

```yaml
image: wazuh/wazuh-manager:4.9.2
image: wazuh/wazuh-indexer:4.9.2
image: wazuh/wazuh-dashboard:4.9.2
```

### 4. Start the Wazuh Single-Node Deployment
You can now start the Wazuh stack.

#### Foreground:
```bash
docker-compose up
```

#### Background:
```bash
docker-compose up -d
```

### 5. Access the Wazuh Dashboard
Once the stack is running, access the Wazuh Dashboard in your browser:

```
http://<your-docker-host-IP>:5601
```

Use the default credentials to log in:
- **Username:** `admin`
- **Password:** `SecretPassword`

For additional security, change the default password after logging in.

### 6. Verify the Setup
Monitor the logs to ensure all components are running smoothly:

```bash
docker-compose logs
```

- Ensure the certificates are correctly placed and configured in the `config/wazuh_indexer_ssl_certs` directory.

## References

- [Wazuh Docker Repository](https://github.com/wazuh/wazuh-docker)
- [Official Wazuh Documentation](https://documentation.wazuh.com/)
