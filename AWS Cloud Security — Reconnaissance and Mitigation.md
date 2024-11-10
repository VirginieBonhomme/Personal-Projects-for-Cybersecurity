# AWS Cloud Security — Reconnaissance and Mitigation

## Objective
This lab demonstrates my ability to identify and mitigate vulnerabilities in a cloud environment using AWS and Kali Linux. I simulated a misconfigured AWS environment, conducted reconnaissance to exploit its vulnerabilities, and applied security measures to secure the system.

---

## 1. AWS Environment Setup

### EC2 Instance Configuration
- **Launched** an EC2 instance (Amazon Linux 2) to simulate a cloud server.
- **Configured** the security group to allow SSH (port 22) and HTTP (port 80) access for testing.
  
![AWSstep1](https://github.com/user-attachments/assets/183dc35d-755b-44ce-a2e7-9cbb52d1042e)

---

### S3 Bucket Configuration
- **Created** an S3 bucket with Block Public Access **disabled** to simulate a common misconfiguration.
- **Uploaded** a file named `secret-data.txt` containing sensitive data and allowed public access to this object.

![AWSstep2](https://github.com/user-attachments/assets/78ed67b4-8a61-4703-b5b9-34f8124067e3)

---

## 2. Kali Linux Reconnaissance

### nmap Scan Results
- **Used `nmap`** from Kali Linux to scan the EC2 instance for open ports and services.
- **Identified** open SSH and HTTP ports.

![AWSstep7](https://github.com/user-attachments/assets/b80349bb-f60b-4077-8d77-cb45cbe9d3df)

---

### AWS CLI S3 Enumeration
- **Enumerated** the S3 bucket from Kali Linux using AWS CLI.
- **Listed** the contents of the vulnerable S3 bucket, confirming public access to `secret-data.txt`.

![AWSstep8](https://github.com/user-attachments/assets/f8954589-9637-4c7a-aa45-a7354ae8edb6)

---

### Exfiltration of Sensitive Data
- **Used AWS CLI** to download the `secret-data.txt` file from the S3 bucket to demonstrate the risk of sensitive data exposure.
- **Displayed** the contents of `secret-data.txt` after exfiltration.

![AWSstep9](https://github.com/user-attachments/assets/e43717ec-1b67-4d41-8a9c-668aceae551f)

---

## 3. Mitigation and Security Hardening

### Before Mitigation:
- The S3 bucket was **publicly accessible**, allowing unauthorized users to list and download objects.

![AWSstep2](https://github.com/user-attachments/assets/1830b6c8-4a91-45ad-8d1e-80db1f9535e3)

---

### After Mitigation:
- **Enabled** Block Public Access for the S3 bucket.
- **Verified** that public access was denied, ensuring no unauthorized access to the bucket or its objects.
- **Tested** access using AWS CLI and browser, both returning `Access Denied`.

![AWSstep3](https://github.com/user-attachments/assets/09758eed-e72d-42fa-bf93-874ae665d266)
![AWSstep5](https://github.com/user-attachments/assets/2cbc1c6c-041b-44cc-9822-5ab7acee9e82)

---

## Key Learnings
- **Cloud Misconfigurations:** Identified how misconfigurations like public S3 buckets can expose sensitive data.
- **Reconnaissance Techniques:** Used `nmap` and AWS CLI for effective cloud reconnaissance.
- **Mitigation Strategies:** Secured the environment by enabling Block Public Access and restricting bucket permissions.
- **Practical Security Skills:** Gained hands-on experience in identifying, exploiting, and securing cloud vulnerabilities.

---

## Conclusion
This lab demonstrates my ability to set up, assess, and secure cloud environments. I showcased practical skills in cloud security, from reconnaissance to mitigation.
