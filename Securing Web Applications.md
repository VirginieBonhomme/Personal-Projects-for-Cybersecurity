# Introduction
When I first started building my Rare Gems web application, which allows users to discover rare sneakers and leave reviews, I didn’t fully understand the importance of security. At the time, I was focused on getting the app to work and didn’t consider how vulnerabilities in the app could lead to data breaches or attacks.

Now that I’ve learned about threat modeling and basic security practices, I realize how critical it is to secure applications from the start. I’m a beginner in both development and cybersecurity, so I’ve been using a framework called PASTA (Process for Attack Simulation and Threat Analysis) to better understand where potential vulnerabilities could occur and how to address them.

Unfortunately, the site is currently down, but you can check out the code on GitHub to see how the app was built:

[Check out the Rare Gems GitHub Repo](https://github.com/VirginieBonhomme/Rare-Gems?tab=readme-ov-file#libraries-and-dependencies)


![Rare Gems](https://github.com/user-attachments/assets/c8b7cdd7-75c0-45a1-ad32-5f33564fa9d0)

![Raregems2](https://github.com/user-attachments/assets/4b28dfe4-1955-41b2-b546-b4b748b01319)

![RGmoblie2](https://github.com/user-attachments/assets/849d8280-3fb9-4be8-bfe4-a9498b0c6d3d)


![RGmobie](https://github.com/user-attachments/assets/981820b1-5f01-484b-a9e8-dcdcd6370537)



# PASTA for Rare Gems Application

| **Stages**                        | **Sneaker Company**                                                                                                                                                             |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **I. Define Business and Security Objectives** | 1. **User Reviews:** The app allows users to create, update, and delete reviews of rare sneakers, ensuring user data integrity is critical.<br>2. **Sneaker Data Storage:** The app uses an internal database to store sneaker information, so protecting this data from unauthorized access is essential.<br>3. **User Authentication:** The app requires secure user authentication and session management to prevent unauthorized access to user accounts. |
| **II. Define the Technical Scope**            | **Technologies used by the application:** <br>1. **Application Flow:** The controller facilitates login functionality, token generation, and user session verification. It interacts with the database (via ActiveRecord) to fetch and authenticate users. <br>2. **Assets:** Sensitive data like username, password, and JWTs generated in the login process are the critical assets that need to be protected.<br>3. **Tech Stack:** Ruby on Rails, ActiveRecord ORM, and Bcrypt for password hashing.  |
| **III. Decompose Application**               |<br>1. ***Entry Points:*** The controller exposes two entry points: POST /auth/login and GET /auth/verify.<br>2. **Trust Boundaries:** User submits data through the login form, it passes through a trust boundary. At this point, it's important to ensure the input is properly validated. |
| **IV. Threat Analysis**                      | **Types of Threats:** <br>1. ***Brute-force Attacks:*** Although the code uses Bcrypt for secure password handling, it could benefit from additional rate-limiting mechanisms to prevent attackers from brute-forcing passwords. <br>2. ***Session hijacking:*** where an attacker could gain unauthorized access to user accounts and manipulate stored data. |
| **V. Vulnerability Analysis**                | **Vulnerabilities:** <br>1. ***Password Strength:*** Enforce strong password requirements and validate password complexity during account creation.<br>2. **Authentication Flaws:** Weak session management, making the app susceptible to session hijacking. |
| **VI. Attack Modeling**                      | **Attack Tree:** <br>- **Root Node: Brute-force Attack on Authentication System** <br> &nbsp;&nbsp;&nbsp;&nbsp; - **Branch 1: Attacker attempts to guess the correct username/password combination** <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - **Sub-Branch: Attacker uses automated scripts to submit multiple login requests** <br> &nbsp;&nbsp;&nbsp;&nbsp; - **Branch 2: Session Hijacking** <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - **Sub-Branch: Weak Login Credentials** |
| **VII. Risk Analysis**            | **Impact**<br>1. ***High Risk:*** Unauthorized access due to weak authentication could expose sensitive data,  <br>2. ***Moderate Risk:*** A JWT token replay attack could allow an attacker to impersonate a user  <br>3. ***Moderate Risk:*** Implement secure session expiration to reduce the risk of session hijacking. |
| **VIII. Security Controls:**            | **Security Controls:** <br>1. ***Rate Limiting:*** Implement rate limiting to prevent brute-force attacks on the login endpoint.<br>2. ***JWT Security:*** Sign JWTs with a strong secret key, set expiration times, and ensure they are transmitted over HTTPS to avoid token hijacking. <br>3. ***Audit Logs:*** Keep track of failed login attempts, token issuances, and verifications for auditing. |

## How I Would Update the Code:

- **Rate Limiting:** Protects against brute-force attacks by limiting the number of login attempts per IP or username.
  
- **MFA (Multi-factor Authentication):** Adds an additional layer of security by requiring a second factor (like a code sent to the user) for login.
  
- **Token Expiration and Rotation:** Ensures that tokens expire after a set time, preventing long-lived sessions, and supports token rotation.
  
- **Audit Logs:** Logs all login attempts, both successful and failed, for auditing and security monitoring purposes.
