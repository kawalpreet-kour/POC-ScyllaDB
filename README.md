# ScyllaDB Proof of Concept (POC) Guide
<img width="200" height="200" alt="scylladnb" src="https://github.com/user-attachments/assets/5bf6d92d-6d18-4910-b71a-1027c3328986" />

---

## Author Information
| Last Updated On | Version | Author           | Level           | Reviewer               |
|-----------------|---------|------------------|-----------------|------------------------|
| 31-07-2025      | V1.0    | Kawalpreet Kour  | Internal Review | Pritam                 |
|                 |         | Kawalpreet Kour  | L0              | Shreya/Sharvani        |
|                 |         | Kawalpreet Kour  | L1              | Abhishek V             |
|                 |         | Kawalpreet Kour  | L2              | Abhishek Dubey/Rishabh Sharma |

---

<details>
  <summary><strong>Table of Contents</strong></summary>

- [Objective](#objective)
- [Pre-requisites](#pre-requisites)
  - [System Requirements](#system-requirements)
  - [Dependencies](#dependencies)
  - [Important Ports](#important-ports)
- [ScyllaDB Installation Guide](#scylladb-installation-guide)
  - [1. Add ScyllaDB APT Repository](#1-add-scylladb-apt-repository)
  - [2. Install ScyllaDB Packages](#2-install-scylladb-packages)
  - [3. Verify Installation](#3-verify-installation)
  - [4. Configure and Run ScyllaDB](#4-configure-and-run-scylladb)
  - [5. Getting Started with ScyllaDB](#5-getting-started-with-scylladb)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [References](#references)

</details>

---

## Objective

The objective of this Proof of Concept (POC) is to illustrate too install and configure ScyllaDB on Ubuntu  with all required dependencies and also helps in setting up a functional ScyllaDB instance for testing or production use.

---

## Pre-requisites

### System Requirements

| Hardware Specifications | Minimum Recommendation |
|-------------------------|------------------------|
| **Processor / Instance Type**     | Dual-Core / t2.medium        |
| **RAM**                 | 4GB                    |
| **Disk**                | 16GB SSD               |
| **OS**                  | Ubuntu 22.04 LTS       |

---
### Dependencies

#### Runtime Dependency

| Name | Version | Description                             |
|------|---------|-----------------------------------------|
| Java | 17      | Required for ScyllaDB driver/script operations |

### Important Ports

| Inbound Traffic | Description                |
|-----------------|----------------------------|
| 9042            | CQL built-in protocol port  |


# ScyllaDB Installation Guide 


## 1. Add ScyllaDB APT Repository

### Create Directory for APT Keyrings
```bash
sudo mkdir -p /etc/apt/keyrings
```
 > Makes a folder to store secure package signing keys


<img width="551" height="58" alt="Screenshot from 2025-08-01 10-36-54" src="https://github.com/user-attachments/assets/599cf0d3-5ad7-49db-9e96-f515e0a0a408" />


### Import ScyllaDB GPG Key
```bash
sudo gpg --homedir /tmp --no-default-keyring --keyring /etc/apt/keyrings/scylladb.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys d0a112e067426ab2
```
> Adds ScyllaDB’s official package key to verify downloads

<img width="1310" height="137" alt="Screenshot from 2025-08-01 10-37-30" src="https://github.com/user-attachments/assets/2a43a61c-fa18-4d54-bbb1-af48b31920fb" />


### Add Repository Configuration File
```bash
sudo wget -O /etc/apt/sources.list.d/scylla.list http://downloads.scylladb.com/deb/debian/scylla-5.4.list
```
> Registers the ScyllaDB package source with your system

<img width="1087" height="259" alt="Screenshot from 2025-08-01 10-38-07" src="https://github.com/user-attachments/assets/79c81659-8267-4cee-8c96-6e15bbe43bc6" />


## 2. Install ScyllaDB Packages

### Install ScyllaDB
```bash
sudo apt-get install -y scylla
```
> Installs ScyllaDB software

<img width="539" height="67" alt="Screenshot from 2025-08-01 10-40-28" src="https://github.com/user-attachments/assets/237f8387-350f-45f4-8824-89148f2b1dcc" />

## 3. Verify Installation

### ScyllaDB Version
```bash
scylla --version
```
> Checks installed versions of ScyllaDB
<img width="360" height="52" alt="Screenshot from 2025-08-01 10-50-21" src="https://github.com/user-attachments/assets/fe1885b9-13b1-4a0c-8140-22dbce2d8ccd" />


## 4. Configure and Run ScyllaDB

### Run Setup Script
```bash
sudo scylla_setup
```
> Helps auto-tune and configure ScyllaDB settings

<img width="981" height="82" alt="Screenshot from 2025-08-01 15-19-04" src="https://github.com/user-attachments/assets/9a20631c-8b52-49d3-9fb1-561735e2de05" />

**Note:** Running scylla_setup is essential after installation. It configures system parameters and installation options.

### Start ScyllaDB as a Service
```bash
sudo systemctl start scylla-server
```
> Starts the ScyllaDB server

<img width="879" height="75" alt="Screenshot from 2025-08-01 10-58-58" src="https://github.com/user-attachments/assets/7b27a123-4227-49ab-82a2-83890f8ef17f" />


### Edit ScyllaDB Configuration File
```bash
sudo vim /etc/scylla/scylla.yaml
```
> Opens the main config file to set IPs

<img width="488" height="37" alt="Screenshot from 2025-08-01 10-59-27" src="https://github.com/user-attachments/assets/1f6b8317-765f-4fa9-ba9b-ffee6879017d" />


### Update These Parameters
```
seeds: "<private ip>"
listen_address: "<private ip>"
rpc_address: "<private ip>"
```

- **seeds**: Private IP used to connect to other Scylla nodes (bootstrapping).
- **listen_address**: Private IP for node-to-node communication.
- **rpc_address**: Private IP for client connections.

### Restart ScyllaDB
```bash
sudo systemctl restart scylla-server
```
> Reloads ScyllaDB with updated settings

<img width="523" height="24" alt="Screenshot from 2025-08-01 15-29-33" src="https://github.com/user-attachments/assets/7ee17ea1-5a1b-4a32-89c3-f872af2bf8b2" />

## 5. Getting Started with ScyllaDB

### Check Node Status
```bash
nodetool status
```
> Shows current status of the Scylla node
---

## Conclusion

This guide provides a complete setup process for running ScyllaDB efficiently on Ubuntu. By following these steps, you’ll have  a working ScyllaDB instance for development or production.

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

