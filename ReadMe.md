# AWS VPC Infrastructure Build — AnyCompany

This repository documents the creation and configuration of a **foundational AWS Virtual Private Cloud (VPC)** architecture for a fictional client, **AnyCompany**.

The objective was to design a **secure, segmented, and highly available network environment**, laying the groundwork for future compute and application deployments.

---

## 🧩 1. Project Overview & Requirements

| Component | Requirement |
| :--- | :--- |
| **VPC CIDR Block** | `10.0.0.0/18` |
| **Public Subnets** | 2 subnets (4,096 IPs each) |
| **Private Subnets** | 2 subnets (4,096 IPs each) |
| **Gateway** | 1 Internet Gateway (IGW) for public connectivity |

Goal: build a modular AWS networking foundation that supports both **external (public)** and **internal (private)** workloads.

---

## ⚙️ 2. Configuration Summary (Step-by-Step)

### **Task 1 — Create the VPC**
- **VPC Name:** `AnyCompany VPC`
- **CIDR:** `10.0.0.0/18`
- **Method:** “VPC only” option in AWS Console

---

### **Task 2 — Create Subnets**
- **Public Subnets (2)**  
  - `AnyCompany subnet public-1` → `10.0.0.0/20`  
  - `AnyCompany subnet public-2` → `10.0.16.0/20`
- **Private Subnets (2)**  
  - `AnyCompany subnet private-1` → `10.0.32.0/20`  
  - `AnyCompany subnet private-2` → `10.0.48.0/20`

Each subnet resides in a separate Availability Zone for high availability.

---

### **Task 3 — Create & Associate Route Tables**
- **Public Route Table:** `AnyCompany public RT`  
  → Associated with public-1 and public-2 subnets  
- **Private Route Table:** `AnyCompany private RT`  
  → Associated with private-1 and private-2 subnets

---

### **Task 4 — Internet Gateway**
- **Name:** `AnyCompany IGW`
- **Action:** Created and attached to `AnyCompany VPC`

---

### **Task 5 — Add Routes**
- **Public Route Table**
  - **Destination:** `0.0.0.0/0`
  - **Target:** `AnyCompany IGW`
- **Private Route Table**
  - Default local traffic only

---

## 🧠 3. Key Concepts Reinforced
- **CIDR Allocation:** Network segmentation using `10.0.0.0/18` → Four `/20` subnets  
- **Subnet Association:** Mapping AZs to specific subnet tiers  
- **Routing Logic:** Public RT → IGW, Private RT → Local  
- **Internet Gateway Integration:** Secure external connectivity

---

## 🚀 4. Next Steps
Future improvements:
- Add **Security Groups** and **Network ACLs**
- Launch **EC2 instances** to validate connectivity
- Automate via **Terraform** or **CloudFormation**

---

*Author: Adonis Madera*  
#AWS #VPC #CloudArchitecture #DevOps #Networking #CloudComputing
