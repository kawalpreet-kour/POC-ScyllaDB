# ScyllaDB Proof of Concept (POC) Guide
<img width="200" height="200" alt="scylladnb" src="https://github.com/user-attachments/assets/5bf6d92d-6d18-4910-b71a-1027c3328986" />

---

## Author Information
| Last Updated On | Version | Author           | Level           | Reviewer               |
|-----------------|---------|------------------|-----------------|------------------------|
| 29-07-2025      | V1.0    | Kawalpreet Kour  | Internal Review | Pritam                 |
|                 |         | Kawalpreet Kour  | L0              | Shreya/Sharvani        |
|                 |         | Kawalpreet Kour  | L1              | Abhishek V             |
|                 |         | Kawalpreet Kour  | L2              | Abhishek Dubey/Rishabh Sharma |

---

<details>
  <summary><strong>Table of Contents</strong></summary>


</details>


## Objective

The objective of this Proof of Concept (POC) is to illustrate the process of installing, configuring, and using ScyllaDB as a standalone instance on an Ubuntu system. It is intended to guide users through the fundamental setup steps, introduce core architectural elements, and demonstrate basic CQL operations and authentication methods. This guide can be used as a hands-on reference for deploying a single-node ScyllaDB environment suitable for development, learning, or initial testing purposes.

---

## Pre-requisites

### System Requirements

| Hardware Specifications | Minimum Recommendation |
|-------------------------|------------------------|
| **Processor / Instance Type**     | Dual-Core / t2.medium        |
| **RAM**                 | 4GB                    |
| **Disk**                | 15GB SSD               |
| **OS**                  | Ubuntu 22.04 LTS       |

---
### Dependencies

#### Runtime Dependency

| Name | Version | Description                             |
|------|---------|-----------------------------------------|
| Java | 17      | For running ScyllaDB driver and scripts |

### Important Ports

| Inbound Traffic | Description                |
|-----------------|----------------------------|
| 9042            | CQL native transport port  |


# ScyllaDB Installation Guide 

## 1. Install OpenJDK 17
ScyllaDB uses Java for its client-side functionality.

```bash
sudo apt install openjdk-17-jre -y
```

## 2. Add ScyllaDB APT Repository

### 2.1 Create Directory for APT Keyrings
```bash
sudo mkdir -p /etc/apt/keyrings
```

### 2.2 Import ScyllaDB GPG Key
```bash
sudo gpg --homedir /tmp --no-default-keyring --keyring /etc/apt/keyrings/scylladb.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys d0a112e067426ab2
```

### 2.3 Add Repository Configuration File
```bash
sudo wget -O /etc/apt/sources.list.d/scylla.list http://downloads.scylladb.com/deb/debian/scylla-5.4.list
```

## 3. Install ScyllaDB Packages

### 3.1 Update Package List
```bash
sudo apt-get update
```

### 3.2 Install ScyllaDB
```bash
sudo apt-get install -y scylla
```

## 4. Verify Installation

### Java Version
```bash
java --version
```

### ScyllaDB Version
```bash
scylla --version
```

## 5. Configure and Run ScyllaDB

### Run Setup Script
This script tunes system settings and configures ScyllaDB.

```bash
sudo scylla_setup
```

**Note:** Running `scylla_setup` is essential after installation. It helps configure system settings and provides installation info.

### Start ScyllaDB as a Service
```bash
sudo systemctl start scylla-server
```

### Edit ScyllaDB Configuration File
```bash
sudo vim /etc/scylla/scylla.yaml
```

### Update These Parameters
```yaml
seeds: "172.31.22.109"
listen_address: "172.31.22.109"
rpc_address: "172.31.22.109"
```

- **seeds**: Private IP used to connect to other Scylla nodes (used for bootstrapping).
- **listen_address**: Private IP where Scylla listens for node-to-node communication.
- **rpc_address**: Private IP for client connections (e.g., CQL, Thrift).

### Restart ScyllaDB
```bash
sudo systemctl restart scylla-server
```

## 6. Getting Started with ScyllaDB

### Check Node Status
```bash
nodetool status
```

### Launch CQL Shell
```bash
cqlsh
```

## 7. Keyspace Operations

### Create Keyspaces
```cql
CREATE KEYSPACE employee_db
WITH REPLICATION = {
  'class': 'SimpleStrategy',
  'replication_factor': 1
};

CREATE KEYSPACE employee_salary
WITH REPLICATION = {
  'class': 'SimpleStrategy',
  'replication_factor': 1
};
```

> These commands create keyspaces with simple replication (adjust as needed).

### List Keyspaces
```cql
DESCRIBE KEYSPACES;
```

### Drop a Keyspace
```cql
DROP KEYSPACE employee_db;
```

## 8. Default Login Credentials
```bash
cqlsh -u cassandra -p cassandra
```

This command connects to ScyllaDB using default credentials (`cassandra`/`cassandra`).

---

## Conclusion

This guide walks through the manual installation and initial configuration of ScyllaDB on Ubuntu 22.04.3 LTS. Follow each step sequentially to set up a working ScyllaDB instance for development or production.

## Contact Information

| Name            | Email                                         |
|-----------------|-----------------------------------------------|
| Kawalpreet Kour | Kawalpreet.kour.snaatak@mygurukulam.co        |

---

## References

| Link                                                                                                            | Description                                |
|-----------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| https://opensource.docs.scylladb.com/stable/getting-started/install-scylla/install-on-linux.html               | Official ScyllaDB Linux Installation Guide |
| https://opensource.docs.scylladb.com/stable/getting-started/system-configuration.html                          | ScyllaDB System Configuration Guide        |

