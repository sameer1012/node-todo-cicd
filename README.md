# Todo List App Deployment

Welcome to the Todo List app deployment guide! This project showcases the end-to-end deployment of a Node.js application using AWS EC2, Docker, and Jenkins.

To get started, launch a new Ubuntu EC2 instance with HTTP and HTTPS ports open. Once your instance is up, update the package list and install OpenJDK 11 with the following commands:
```bash
sudo apt update
sudo apt install openjdk-11-jre
Verify the Java installation by running:
java -version
Next, install Jenkins by adding the official Jenkins key and repository:
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

Start Jenkins and check its status:
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins

Update your EC2 security group to allow inbound traffic on port 8080 for Jenkins access. Retrieve the Jenkins initial admin password:
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Follow the Jenkins setup wizard to install suggested plugins and create admin credentials.

In Jenkins, create a new item for your project. Configure the build to pull code from GitHub by adding your GitHub repository URL. Generate an SSH key pair on your EC2 instance:
ssh-keygen

Add the public key (id_rsa.pub) to your GitHub account under SSH keys. Build and run your Node.js app using Docker. Install Docker:
sudo apt install docker.io

Create a Dockerfile in your project directory with the following content:
FROM node:12.2.0-alpine
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 8000
CMD ["node", "app.js"]

Build and run your Docker container:
docker build -t todo-node-app .
docker run -d --name node-todo-app -p 8000:8000 todo-node-app

Update your EC2 security group to allow inbound traffic on port 8000 for app access. To automate builds and deployments with Jenkins, add build steps to the Jenkins job configuration:
docker build -t node-app-todo .
docker run -d --name node-app-container -p 8000:8000 node-app-todo

Save your changes and run the build. Address any permission issues by adding the Jenkins user to the Docker group:
sudo usermod -a -G docker jenkins
sudo systemctl restart jenkins


Finally, configure GitHub webhooks in Jenkins to automatically trigger builds on code updates. Install the necessary Jenkins plugin for webhooks and complete the setup.

Your Todo List app should now be up and running, accessible at http://<your-ec2-public-ip>:8000. This project demonstrates practical skills in cloud deployment, Docker containerization, and CI/CD automation using Jenkins.


Feel free to modify any details according to your specific setup or preferences!



