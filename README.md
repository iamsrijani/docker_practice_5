Program 5: Demonstrate CI Integration by Running Jenkins in a Docker Container
Aim

To demonstrate Continuous Integration (CI) integration by running Jenkins in a Docker container.

Step 1: Create a Docker Directory
bash
mkdir program5
cd program5
Step 2: Create a Dockerfile

Create a file named Dockerfile and add the following code:

Dockerfile
FROM jenkins/jenkins:lts

USER root

RUN apt-get update && apt-get install -y docker.io

USER jenkins




Step 3: Create docker-compose.yml

Create a file named docker-compose.yml and add the following code:

version: "3.8"
services:
  jenkins:
    build: .
    container_name: jenkins-ci
    ports:
      - "8080:8080"
      - "50000:50000"
    volumes:
      - jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock

volumes:
  jenkins_home:


Step 4: Build and Run the Container
bash
docker compose up -d --build
Step 5: Verify the Container
bash
docker ps


The Jenkins container should appear with the name jenkins-ci.

Step 6: Open Jenkins in Browser

Open the following URL:

text
http://localhost:8080
Step 7: Get the Initial Jenkins Password

Execute the following command:

bash
docker exec jenkins-ci cat /var/jenkins_home/secrets/initialAdminPassword


Copy the generated password.

Step 8: Configure Jenkins
Open http://localhost:8080 in your browser.
Enter the initial administrator password.
Click Continue.
Select Install suggested plugins.
Create an administrator user.
Complete the Jenkins setup.
Step 9: Verify Jenkins

After successful installation, the Jenkins dashboard will appear in the browser.

Step 10: View Container Logs
bash
docker logs jenkins-ci
Step 11: Stop the Container
bash
docker compose down
Folder Structure
text
program5/
│
├── Dockerfile
│
└── docker-compose.yml
Expected Output
Jenkins container runs successfully.
Container name: jenkins-ci
Jenkins URL: http://localhost:8080
Jenkins dashboard is displayed successfully.
Jenkins setup is completed.
Result

Thus, CI integration was successfully demonstrated by running Jenkins in a Docker container.
