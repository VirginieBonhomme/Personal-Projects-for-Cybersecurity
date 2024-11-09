# Network-Reconnaissance-and-Server-Hardening-Usi

**Objective**: Set up and secure a simple HTTP server to simulate real-world web server environments, analyze network traffic, and enhance penetration testing skills.

**Key Highlights**:

- **Network Reconnaissance**: 
  - Conducted service discovery using `nmap` to identify open ports and assess the Target machine's exposure.
  - Leveraged `netcat` for manual testing of network services, confirming the accessibility of open ports.

- **Web Server Deployment**:
  - Configured a Python-based HTTP server to simulate a web service, exploring different binding options (`0.0.0.0` vs. `127.0.0.1`).
  - Performed local and external access testing to validate server configurations and access controls.

- **Exploitation and Data Retrieval**:
  - Used `curl` to retrieve sensitive files (`.bashrc`), demonstrating the risks of misconfigured servers.
  - Evaluated the server's response to manual HTTP requests to understand potential vulnerabilities.

- **Security Hardening**:
  - Reconfigured the server to restrict access by binding it to `localhost` and limiting its exposure to external connections.
  - Simulated real-world scenarios of unauthorized access attempts and analyzed the results.

**Tools and Techniques**:
- **Nmap**: Network scanning and service enumeration.
- **Netcat**: Manual testing of network connections.
- **Curl**: Fetching files and testing HTTP requests.
- **Python**: Quick deployment of an HTTP server for testing.

**Key Achievements**:
- Gained hands-on experience in setting up, testing, and securing a basic HTTP server.
- Strengthened knowledge of network configurations and common security risks.
- Developed practical skills in penetration testing and vulnerability assessment.

**Impact**:
- Improved ability to identify and mitigate misconfigurations in web server environments.
- Enhanced understanding of core cybersecurity principles, including the importance of securing exposed services.
- Demonstrated proficiency with essential tools and techniques used in cybersecurity operations.

This project showcases my ability to simulate and analyze real-world cybersecurity scenarios, reinforcing my technical expertise and problem-solving skills.
