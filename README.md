# Practice Program 5 – Demonstrating CI Integration by Running Jenkins in a Docker Container

## Aim

To demonstrate Continuous Integration (CI) integration by installing and running Jenkins inside a Docker container using Docker Compose.

## Technologies Used

- **Jenkins:** Automation server used for Continuous Integration.
- **Docker:** Platform used to create and run containers.
- **Docker Compose:** Tool used to define and manage multi-container Docker applications.
- **Dockerfile:** File containing instructions to build a Docker image.
- **Ubuntu:** Operating system used for implementation.
- **Browser:** Used to access the Jenkins dashboard.

## Description

This program demonstrates how to set up Jenkins inside a Docker container using Docker Compose.

Jenkins is an open-source automation server widely used for Continuous Integration and Continuous Delivery (CI/CD).

Docker is used to containerize Jenkins, while Docker Compose simplifies the process of building, starting and managing the Jenkins container.

The Jenkins container is configured with persistent storage to retain its data, configuration and installed plugins.

The Jenkins dashboard can be accessed through a web browser using port 8080.

---

## Prerequisites

- Ubuntu Linux
- Docker installed and running
- Docker Compose plugin installed
- Internet connection
- Web browser

Verify Docker installation:

```bash
docker --version
```

Verify Docker Compose:

```bash
docker compose version
```

If Docker is not running, start it:

```bash
sudo systemctl start docker
```

Enable Docker to start automatically:

```bash
sudo systemctl enable docker
```

---

## Step 1: Create a Docker Directory

Create a directory named `program5`:

```bash
mkdir program5
```

Navigate into the directory:

```bash
cd program5
```

Check the current directory:

```bash
pwd
```

## Step 2: Create the Dockerfile

Create a file named `Dockerfile`:

```bash
nano Dockerfile
```

Add the following code:

```dockerfile
FROM jenkins/jenkins:lts

USER root

RUN apt-get update && apt-get install -y docker.io

USER jenkins
```

Save the file using:

1. Press `Ctrl + O`.
2. Press Enter.
3. Press `Ctrl + X`.

### Dockerfile Explanation

| Instruction | Description |
|---|---|
| FROM | Specifies the Jenkins Long-Term Support (LTS) base image. |
| USER root | Switches to the root user to perform administrative operations. |
| RUN | Updates package repositories and installs Docker inside the Jenkins image. |
| USER jenkins | Switches back to the Jenkins user for running Jenkins. |

## Step 3: Create docker-compose.yml

Create the Docker Compose configuration file:

```bash
nano docker-compose.yml
```

Add the following configuration:

```yaml
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
```

Save and exit the file.

### Docker Compose Explanation

| Instruction | Description |
|---|---|
| version | Specifies the Docker Compose file format version. |
| services | Defines the services used in the application. |
| jenkins | Defines the Jenkins service. |
| build: . | Builds the Docker image using the Dockerfile in the current directory. |
| container_name | Assigns the container name `jenkins-ci`. |
| 8080:8080 | Maps host port 8080 to container port 8080 for the Jenkins web interface. |
| 50000:50000 | Maps port 50000 for Jenkins inbound agent connections. |
| volumes | Defines persistent storage and shared resources. |
| jenkins_home | Stores Jenkins data, configurations and plugins persistently. |
| docker.sock | Allows the Jenkins container to communicate with the host Docker daemon through its Docker socket. |

## Step 4: Build and Run the Jenkins Container

Execute the following command:

```bash
docker compose up -d --build
```

### Command Explanation

- `docker compose`: Executes Docker Compose commands.
- `up`: Creates and starts the defined services.
- `-d`: Runs the container in detached mode.
- `--build`: Builds the Docker image before starting the container.

Docker will download the required Jenkins image, build the custom image and start the Jenkins container.

## Step 5: Verify the Running Container

Execute:

```bash
docker ps
```

This command displays all currently running Docker containers.

The output should show:

- Container name: `jenkins-ci`
- Image: Jenkins-based custom image
- Status: Up
- Port mapping: 8080 and 50000

To view all containers, including stopped ones:

```bash
docker ps -a
```

## Step 6: Open Jenkins in a Browser

Open a web browser and enter:

```text
http://localhost:8080
```

The Jenkins setup and unlock page should appear.

This confirms that Jenkins is accessible through the mapped host port.

## Step 7: Get the Initial Jenkins Administrator Password

Jenkins generates an initial administrator password during its first setup.

Execute:

```bash
docker exec jenkins-ci cat /var/jenkins_home/secrets/initialAdminPassword
```

Copy the password displayed in the Terminal.

This password is required to unlock Jenkins.

## Step 8: Configure Jenkins

Open:

```text
http://localhost:8080
```

Follow these steps:

1. Enter the initial administrator password.
2. Click **Continue**.
3. Select **Install suggested plugins**.
4. Wait for the plugins to install.
5. Create an administrator user.
6. Enter the required username and password.
7. Complete the Jenkins setup.
8. Start using Jenkins.

After configuration, the Jenkins dashboard will appear.

## Step 9: Verify Jenkins Installation

Once the setup is completed, the Jenkins dashboard should be visible in the browser.

The dashboard confirms that Jenkins has been successfully installed and configured.

Jenkins can now be used to create jobs and configure automation pipelines.

## Step 10: View Jenkins Container Logs

To view the Jenkins container logs, execute:

```bash
docker logs jenkins-ci
```

To continuously monitor the logs:

```bash
docker logs -f jenkins-ci
```

This helps identify startup messages, errors and other information generated by Jenkins.

Press `Ctrl + C` to stop viewing continuous logs.

## Step 11: Stop the Jenkins Container

Navigate to the project directory if necessary:

```bash
cd ~/program5
```

Stop and remove the Jenkins container and Compose-created network:

```bash
docker compose down
```

The named volume `jenkins_home` is preserved, so Jenkins configuration and data remain available when the container is started again.

To start Jenkins again:

```bash
docker compose up -d
```

---

## Project Structure

```text
program5/
│
├── Dockerfile
│
└── docker-compose.yml
```

## Important Docker Commands

| Command | Description |
|---|---|
| `docker --version` | Displays the installed Docker version. |
| `docker compose version` | Displays the Docker Compose version. |
| `docker compose up -d --build` | Builds and starts the Jenkins container in detached mode. |
| `docker ps` | Displays running containers. |
| `docker ps -a` | Displays all containers. |
| `docker logs jenkins-ci` | Displays Jenkins container logs. |
| `docker logs -f jenkins-ci` | Continuously displays container logs. |
| `docker exec jenkins-ci ...` | Executes a command inside the Jenkins container. |
| `docker compose down` | Stops and removes Compose-managed containers and networks. |
| `docker compose up -d` | Starts the Jenkins service again. |

## Important Jenkins Concepts

| Concept | Description |
|---|---|
| Jenkins | Open-source automation server. |
| Continuous Integration (CI) | Practice of frequently integrating code changes into a shared repository and validating them through automated builds and tests. |
| Continuous Delivery (CD) | Practice of keeping software ready for release through automated processes. |
| Jenkins Dashboard | Web interface used to manage Jenkins. |
| Jenkins Plugins | Extensions that add functionality to Jenkins. |
| Jenkins Agent | Machine or process that executes Jenkins build tasks. |
| Persistent Volume | Storage used to retain Jenkins data even when its container is removed. |
| Docker Compose | Tool used to define and manage Docker services. |

## Expected Output

After successfully executing the program:

1. The Jenkins Docker image is built successfully.
2. The Jenkins container runs with the name `jenkins-ci`.
3. Jenkins is accessible through:

   `http://localhost:8080`

4. The initial administrator password is retrieved successfully.
5. Suggested plugins are installed.
6. An administrator account is created.
7. The Jenkins dashboard is displayed successfully.
8. Jenkins is ready for creating and managing CI jobs.

## Result

Thus, Continuous Integration (CI) integration was successfully demonstrated by running Jenkins inside a Docker container using Docker Compose.

The Jenkins container was built and executed successfully, the initial administrator password was retrieved, and the Jenkins setup was completed through the web browser.

