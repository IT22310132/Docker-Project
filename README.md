
# **Containerized Network Services using Docker**

This project implements essential network services — **DNS**, **DHCP**, and **Proxy** — using Docker containers on **Ubuntu 24.04 LTS**.
Each service is packaged into a custom Docker image, configured with automated build scripts, and orchestrated using **Docker Compose** for streamlined deployment.

---

## 🚀 **Project Overview**

The objective of this project is to deploy core network infrastructure components as lightweight, isolated Docker containers.
The solution demonstrates:

* Docker-based service deployment
* Custom image creation
* Automated configuration using Dockerfiles
* Verification through live testing
* Centralized orchestration via Docker Compose
* Image publishing via Docker Hub

---

## 📦 **Services Implemented**

### **1. DNS Server (BIND9)**

* Custom zone: `csne.vcct.com`
* Configured global DNS options, zones, and resource records
* Dockerfile automates BIND9 installation & config file placement
* Tested using `dig` from a separate host

### **2. DHCP Server (ISC-DHCP)**

* Configured IP pool, subnet, lease times, router & DNS options
* Dockerfile builds a DHCP container with required configs
* Verified DHCPDISCOVER → DHCPOFFER → DHCPREQUEST → DHCPACK messages
* Successfully issued dynamic IPs to client hosts

### **3. Proxy Server (Squid)**

* Implemented HTTP caching and access control
* Custom `squid.conf` and `entrypoint.sh` for clean startup
* Verified caching behaviour using repeated `curl` requests
* Observed TCP_MISS and TCP_MEM_HIT entries in access logs

---

## 🏗️ **Project Structure**

```
vcct/
│── dns-server/
│   ├── Dockerfile
│   ├── named.conf.options
│   ├── named.conf.local
│   └── db.csne.vcct.com
│
│── dhcp-server/
│   ├── Dockerfile
│   └── dhcpd.conf
│
│── proxy-server/
│   ├── Dockerfile
│   ├── squid.conf
│   └── entrypoint.sh
│
└── docker-compose.yml
```

---

## 🛠️ **How to Build & Run**

### **Build Images**

```bash
cd dns-server
docker build -t username/dns-server .

cd ../dhcp-server
docker build -t username/dhcp-server .

cd ../proxy-server
docker build -t username/proxy-server .
```

### **Run Containers Individually**

```bash
docker run -d --name dns-server username/dns-server
docker run -d --name dhcp-server --network host username/dhcp-server
docker run -d --name proxy-server --network host username/proxy-server
```

### **Or Use Docker Compose**

```bash
docker-compose up -d
```

---

## 🌐 **Docker Hub Integration**

All built images were pushed to Docker Hub using:

```bash
docker push username/image-name
```

Images can be pulled on any system for quick deployment:

```bash
docker pull username/image-name
```

---

## ✅ **Testing**

* **DNS:** Verified domain <csne.vcct.com> and internet lookups using `dig`
* **DHCP:** Successfully issued leases to clients after release/renew
* **Proxy:** Validated caching with Squid access logs and faster repeated loads

---

## 📋 **Technologies Used**

* **Ubuntu 24.04 LTS**
* **Docker Engine**
* **Docker Compose**
* **BIND9 DNS**
* **ISC-DHCP**
* **Squid Proxy**
* **Docker Hub**

---

## 📄 **Conclusion**

This project demonstrates how Docker can efficiently host and isolate core network infrastructure components. By leveraging Dockerfiles, automated configuration, and Docker Compose, the environment becomes portable, scalable, and easy to manage across multiple systems.
