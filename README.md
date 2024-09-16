# Todo List App Deployment

Welcome to the Todo List app deployment guide! This project demonstrates the end-to-end deployment of a Node.js application using AWS EC2, Docker, and Jenkins.

## Getting Started

1. **Launch a New EC2 Instance**

   - Create a new Ubuntu EC2 instance with HTTP and HTTPS ports open.

2. **Install Java and Jenkins**

   - Update the package list and install OpenJDK 11:
     ```bash
     sudo apt update
     sudo apt install openjdk-11-jre
     ```
   - Verify Java installation:
     ```bash
     java -version
     ```
   - Install Jenkins:
     ```bash
     sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
     echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
     sudo apt-get update
     sudo apt-get install jenkins
     ```


3. **Start and Verify Jenkins**

   - Enable, start, and check Jenkins status:
     ```bash
     sudo systemctl enable jenkins
     sudo systemctl start jenkins
     sudo systemctl status jenkins
     ```

4. **Configure EC2 Security Group**

   - Allow inbound traffic on port 8080 to access Jenkins.

5. **Retrieve Jenkins Admin Password**

   - Get the initial admin password:
     ```bash
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```

   - Follow the Jenkins setup wizard to install suggested plugins and create admin credentials.

6. **Configure Jenkins for Your Project**

   - In Jenkins, create a new item for your project and configure the build to pull code from GitHub by adding your GitHub repository URL.
   - Generate an SSH key pair on your EC2 instance:
     ```bash
     ssh-keygen
     ```
   - Add the public key (`id_rsa.pub`) to your GitHub account under SSH keys.

7. **Set Up Docker**

   - Install Docker:
     ```bash
     sudo apt install docker.io
     ```
   - Create a `Dockerfile` in your project directory:
     ```dockerfile
     FROM node:12.2.0-alpine
     WORKDIR /app
     COPY . .
     RUN npm install
     EXPOSE 8000
     CMD ["node", "app.js"]
     ```
   - Build and run your Docker container:
     ```bash
     docker build -t todo-node-app .
     docker run -d --name node-todo-app -p 8000:8000 todo-node-app
     ```

8. **Update EC2 Security Group**

   - Allow inbound traffic on port 8000 to access the app.

9. **Automate Builds and Deployments with Jenkins**

   - In Jenkins, add build steps to the job configuration:
     ```bash
     docker build -t node-app-todo .
     docker run -d --name node-app-container -p 8000:8000 node-app-todo
     ```
   - Save the changes and run the build.

   - If you encounter permission issues, add the Jenkins user to the Docker group:
     ```bash
     sudo usermod -a -G docker jenkins
     sudo systemctl restart jenkins
     ```

10. **Configure GitHub Webhooks**

    - Set up GitHub webhooks in Jenkins to automatically trigger builds on code updates.
    - Install the necessary Jenkins plugin for webhooks and complete the setup.

## Access the Application

Your Todo List app should now be accessible at `http://<your-ec2-public-ip>:8000`. This project demonstrates practical skills in cloud deployment, Docker containerization, and CI/CD automation using Jenkins.

Feel free to modify any details according to your specific setup or preferences!
![image](https://github.com/user-attachments/assets/57ff956d-b657-4c9f-8c9e-2da38ea84df5)


