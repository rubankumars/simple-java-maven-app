# Simple Java Maven App CI/CD Pipeline

This project is a Java application built with Maven and integrated with Jenkins and SonarQube for automated testing and code quality analysis.

## 🛠 Prerequisites

1.  **Jenkins Server**: Installed and running.
2.  **SonarQube Server**: Installed and running (Local or Cloud).
3.  **Java & Maven**: Installed on the Jenkins node (if not using Docker).

## 🚀 Setup Instructions

### Step 1: Install Jenkins Plugins
1.  Navigate to **Manage Jenkins** > **Manage Plugins**.
2.  Install the following plugins:
    *   **SonarQube Scanner for Jenkins**
    *   **Pipeline**
    *   **Git**

### Step 2: Generate SonarQube Token
1.  Log in to **SonarQube**.
2.  Go to **My Account** (top right) > **Security**.
3.  Under **Tokens**, enter a name (e.g., `jenkins-token`) and click **Generate**.
4.  **Important**: Copy the token immediately; you won't see it again.

### Step 3: Configure Credentials in Jenkins
1.  Go to **Manage Jenkins** > **Credentials** > **System** > **Global credentials**.
2.  Click **Add Credentials**:
    *   **Kind**: Secret text
    *   **Secret**: [Paste your SonarQube Token]
    *   **ID**: `sonar-token`
    *   **Description**: SonarQube Authentication Token

### Step 4: Configure SonarQube Server in Jenkins
1.  Go to **Manage Jenkins** > **System** (or **Configure System**).
2.  Find **SonarQube servers** and click **Add SonarQube**.
3.  **Name**: `SonarQube` (This must match the name in your Jenkinsfile).
4.  **Server URL**: `http://<your-sonarqube-url>:9000`.
5.  **Server authentication token**: Select `sonar-token` from the dropdown.

### Step 5: Configure Build Tools (Non-Docker Setup)
1.  Go to **Manage Jenkins** > **Tools**.
2.  **JDK**: Add JDK and name it `Java 11`.
3.  **Maven**: Add Maven and name it `Maven 3.x`.
4.  **SonarQube Scanner**: Under **SonarQube Scanner installations**, click **Add SonarQube Scanner**, name it `SonarScanner`, and check **Install automatically**.

### Step 6: Configure SonarQube Webhook (For Quality Gate)
1.  In **SonarQube**, go to **Administration** > **Configuration** > **Webhooks**.
2.  Click **Create**:
    *   **Name**: `Jenkins Webhook`
    *   **URL**: `http://<YOUR_JENKINS_URL>/sonarqube-webhook/` (The trailing slash is required).

### Step 7: Create and Configure the Jenkins Job
1. On the Jenkins Dashboard, click **New Item**.
2. Enter name `simple-java-maven-app` and select **Pipeline**, then click **OK**.
3. Scroll down to the **Pipeline** section and configure:
    * **Definition**: `Pipeline script from SCM`
    * **SCM**: `Git`
    * **Repository URL**: `https://github.com`
    * **Branch Specifier**: `*/master`
    * **Script Path**: `jenkins/Jenkinsfile` <-- *(Crucial: Since your file is inside the jenkins folder)*
4. Click **Save**.

### Step 8: Run the Pipeline
1. Click **Build Now** on the left sidebar.
2. **Monitor the Stages**:
    * **Build**: Compiles the app.
    * **Test**: Runs JUnit tests and records results.
    * **SonarQube Analysis**: Sends code data to your SonarQube server.
    * **Quality Gate**: Jenkins will pause and wait for a "Go/No-Go" signal from SonarQube via the Webhook.
    * **Deliver**: Executes the delivery script located at `jenkins/scripts/deliver.sh`.

## 📊 Viewing Results
* **Test Reports**: After a build, click on the **Latest Test Result** in Jenkins to see passing/failing tests.
* **Sonar Dashboard**: A SonarQube icon will appear on the Jenkins Job page; click it to see detailed code smells, bugs, and security vulnerabilities.
* **Code Coverage**: Detailed line-by-line coverage is available within the SonarQube UI under the "Measures" tab.

## 📁 Repository Structure
* `src/`: Main Java source code and tests.
* `jenkins/Jenkinsfile`: The CI/CD pipeline definition (Non-Docker version).
* `jenkins/scripts/`: Automation scripts for deployment/delivery.
* `pom.xml`: Maven configuration including JaCoCo and SonarScanner plugins.

## 🏗 Pipeline Execution
The pipeline is defined in the `Jenkinsfile`. It will:
1.  Build the project using Maven.
2.  Run Unit Tests.
3.  Execute SonarQube analysis.
4.  Wait for the Quality Gate result.
5.  Deliver the application if all checks pass.

This repository is for the
[Build a Java app with Maven](https://jenkins.io/doc/tutorials/build-a-java-app-with-maven/)
tutorial in the [Jenkins User Documentation](https://jenkins.io/doc/).

The repository contains a simple Java application which outputs the string
"Hello world!" and is accompanied by a couple of unit tests to check that the
main application works as expected. The results of these tests are saved to a
JUnit XML report.

The `jenkins` directory contains an example of the `Jenkinsfile` (i.e. Pipeline)
you'll be creating yourself during the tutorial and the `scripts` subdirectory
contains a shell script with commands that are executed when Jenkins processes
the "Deliver" stage of your Pipeline..
