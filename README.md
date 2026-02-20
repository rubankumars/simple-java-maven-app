# Maven Java Project with Jenkins & SonarQube CI/CD

This repository contains a sample Java application managed by a Maven build system and a fully automated Jenkins Pipeline.

## 🚀 Pipeline Features
- **Build**: Compiles the source code and packages it into a JAR file.
- **Unit Testing**: Executes JUnit tests and publishes results to Jenkins.
- **Static Code Analysis**: Integrates with [SonarQube](https://www.sonarqube.org) to detect bugs, vulnerabilities, and code smells.
- **Quality Gate**: Automatically fails the build if the code does not meet pre-defined quality standards.
- **Delivery**: Executes deployment scripts upon successful validation.

## 🛠 Prerequisites

### 1. Jenkins Configuration
*   **Plugins**: Install the [SonarQube Scanner for Jenkins](https://plugins.jenkins.io) plugin.
*   **SonarQube Server**: Configure your server under `Manage Jenkins` > `System`. Ensure the name matches `'SonarQube'` as used in the Jenkinsfile.
*   **Webhook**: In SonarQube, go to `Administration` > `Configuration` > `Webhooks` and add a webhook pointing to `YOUR_JENKINS_URL/sonarqube-webhook/` to enable the Quality Gate wait step.

### 2. Docker
The pipeline uses a Docker agent (`maven:3-alpine`) to ensure a consistent build environment. Ensure the Jenkins node has Docker installed and the Jenkins user has permission to run Docker commands.

## 📁 Project Structure
- `src/`: Java source code.
- `Jenkinsfile`: Defines the CI/CD pipeline stages.
- `jenkins/scripts/`: Contains delivery/deployment automation scripts.
- `pom.xml`: Maven project configuration.

## 🚦 How to Run
1. Create a new **Pipeline** job in Jenkins.
2. Under **Pipeline Script from SCM**, select **Git** and provide this repository URL.
3. Ensure the script path is set to `Jenkinsfile`.
4. Click **Build Now**.


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
