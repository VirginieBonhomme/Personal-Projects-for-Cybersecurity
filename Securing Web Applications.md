# Introduction
When I first started building my Rare Gems web application, which allows users to discover rare sneakers and leave reviews, I didn’t fully understand the importance of security. At the time, I was focused on getting the app to work and didn’t consider how vulnerabilities in the app could lead to data breaches or attacks.

Now that I’ve learned about threat modeling and basic security practices, I realize how critical it is to secure applications from the start. I’m a beginner in both development and cybersecurity, so I’ve been using a framework called PASTA (Process for Attack Simulation and Threat Analysis) to better understand where potential vulnerabilities could occur and how to address them.

Unfortunately, the site is currently down, but you can check out the code on GitHub to see how the app was built:

[Check out the Rare Gems GitHub Repo](https://github.com/VirginieBonhomme/Rare-Gems?tab=readme-ov-file#libraries-and-dependencies)


![Rare Gems](https://github.com/user-attachments/assets/c8b7cdd7-75c0-45a1-ad32-5f33564fa9d0)

![Raregems2](https://github.com/user-attachments/assets/4b28dfe4-1955-41b2-b546-b4b748b01319)

![RGmoblie2](https://github.com/user-attachments/assets/849d8280-3fb9-4be8-bfe4-a9498b0c6d3d)


![RGmobie](https://github.com/user-attachments/assets/981820b1-5f01-484b-a9e8-dcdcd6370537)

![PASTA data flow diagram](https://github.com/user-attachments/assets/1a92b905-0a82-44b2-9203-69dc9898376a)

![PASTA attack tree](https://github.com/user-attachments/assets/d306a075-38b1-4022-8c27-910717703cd8)



# PASTA for Rare Gems Application

| **Stages**                        | **Sneaker Company**                                                                                                                                                             |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **I. Define Business and Security Objectives** | 1. **User Reviews:** The app allows users to create, update, and delete reviews of rare sneakers, ensuring user data integrity is critical.<br>2. **Sneaker Data Storage:** The app uses an internal database to store sneaker information, so protecting this data from unauthorized access is essential.<br>3. **User Authentication:** The app requires secure user authentication and session management to prevent unauthorized access to user accounts. |
| **II. Define the Technical Scope**            | **Technologies used by the application:** <br>1. SQL Database <br>2. Ruby on Rails API with RESTful JSON endpoints <br>3. Web Application Interface <br> **Why prioritize these technologies?** <br> The SQL database is prioritized for efficiently managing and storing sneaker information and user reviews. Ruby on Rails API is prioritized for backend functionality, exposing RESTful JSON endpoints to interact with the front-end and manage data. The web application interface is crucial for user interaction, allowing them to browse sneakers and manage reviews. |
| **III. Decompose Application**               | **Data Flow Diagram:** <br>1. **User → Web App:** User searches for rare sneakers or submits a review. <br>2. **Web App → Database:** Fetches sneaker information or stores user reviews. |
| **IV. Threat Analysis**                      | **Types of Threats:** <br>1. **Internal Threats:** SQL injection attacks that could compromise the integrity of user reviews and sneaker data. <br>2. **External Threats:** Session hijacking, where an attacker could gain unauthorized access to user accounts and manipulate stored data. |
| **V. Vulnerability Analysis**                | **Vulnerabilities:** <br>1. **Code Vulnerabilities:** Lack of prepared statements in SQL queries, which could lead to SQL injection attacks. <br>2. **Authentication Flaws:** Weak session management, making the app susceptible to session hijacking. |
| **VI. Attack Modeling**                      | **Attack Tree:** <br>- **Root Node: User Data** <br> &nbsp;&nbsp;&nbsp;&nbsp; - **Branch 1: SQL Injection** <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - **Sub-Branch: Lack of Prepared Statements** <br> &nbsp;&nbsp;&nbsp;&nbsp; - **Branch 2: Session Hijacking** <br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - **Sub-Branch: Weak Login Credentials** |
| **VII. Risk Analysis and Impact**            | **Security Controls:** <br>1. Prepared Statements: Prevent SQL injection attacks by securing database queries. <br>2. Multi-Factor Authentication (MFA): Add an extra layer of security for user authentication. <br>3. Input Validation and Sanitization: Prevent XSS and other input-based attacks. <br>4. Session Timeout and Management: Implement secure session expiration to reduce the risk of session hijacking. |


## Explanation of Old Code:
In the original code, the AuthenticationsController handles user login by finding the user based on the username and then verifying the password using @user.authenticate. If the credentials are valid, a token is generated and returned. However, there are a few potential issues: the code doesn’t handle the case where @user is nil, which could lead to an error, and the error message returned is too specific, which could give an attacker clues about whether the username or password was incorrect. Additionally, the token generation isn't detailed, which could pose security risks if not implemented properly.


```bash
class AuthenticationsController < ApplicationController
  before_action :authorize_request, except: :login

  # POST /auth/login
  def login
    @user = User.find_by(username: login_params[:username])
    if @user.authenticate(login_params[:password]) #authenticate method provided by Bcrypt and 'has_secure_password'
      @token = encode({id: @user.id})
      render json: {
        user: @user.attributes.except("password_digest"),
        token: @token
        }, status: :ok
    else
      render json: { errors: 'unauthorized' }, status: :unauthorized
    end
  end
  
  # GET /auth/verify
  def verify
    render json: @current_user.attributes.except("password_digest"), status: :ok
  end


  private

  def login_params
    params.require(:authentication).permit(:username, :password)
  end
end
```

### Updated Code
```bash
class AuthenticationsController < ApplicationController
  before_action :authorize_request, except: :login

  # POST /auth/login
  def login
    @user = User.find_by(username: login_params[:username].strip) # sanitize input
    if @user&.authenticate(login_params[:password]) # authenticate with BCrypt
      @token = encode({ id: @user.id }) # Add expiration in token encoding
      render json: {
        user: @user.attributes.except("password_digest"),
        token: @token
        }, status: :ok
    else
      render json: { errors: 'unauthorized' }, status: :unauthorized
    end
  end
  
  # GET /auth/verify
  def verify
    render json: @current_user.attributes.except("password_digest"), status: :ok
  end


  private

  def login_params
    params.require(:authentication).permit(:username, :password).tap do |params|
      params[:username] = params[:username].strip # sanitize input
    end
  end

  # Encoding method with expiration
  def encode(payload)
    payload[:exp] = 24.hours.from_now.to_i # Set token to expire in 24 hours
    JWT.encode(payload, 'your_secret_key') # Use a real secret key here
  end
end
```


## How I Would Update the Code:

- **Introduce Safe Navigation Operator (`@user&.authenticate`)**:  
  I would use the safe navigation operator (`@user&.authenticate`) to safely handle cases where `@user` is `nil`. This prevents the app from throwing an error when trying to call `authenticate` on a `nil` object if the username doesn’t exist.
  
- **Update Error Messages**:  
 Instead of returning a specific error like `unauthorized`, I would replace it with a more generic error message such as `Invalid credentials`. This approach makes it harder for attackers to figure out whether it’s the username or password that is incorrect, improving security by preventing credential-based attacks.

- **Secure Token Handling with JWT**:  
For the token, I would ensure that the encoding uses a secure method, such as JSON Web Tokens (JWT). I’d implement token generation with a secret key, setting an expiration time to enhance security. This would involve updating the encode method to securely encrypt the token and ensure that it can be safely decoded when needed.

