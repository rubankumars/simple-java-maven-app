# 🚀 Java Maven CI/CD Pipeline (Jenkins + SonarQube + Aikido)

This repository contains a Java application managed by Maven, featuring a complete CI/CD pipeline defined in `jenkins/Jenkinsfile`.

## 🛠 Prerequisites

1. **Jenkins Server**: Installed with the following plugins:
    * `SonarQube Scanner`
    * `Pipeline`
    * `Git`
2. **SonarQube Server**: Local or Cloud instance.
3. **Aikido Security**: An account at [Aikido.dev](https://app.aikido.dev).
4. **Jenkins Node Tools**: JDK 11, Maven 3.x, and Node.js (for Aikido CLI) installed.

---

## ⚙️ Step 1: Global Tool Configuration (Jenkins)
Go to **Manage Jenkins** > **Tools** and configure:
* **JDK**: Name it `Java 11`.
* **Maven**: Name it `Maven 3.x`.
* **SonarQube Scanner**: Add a scanner named `SonarScanner`.

---

## 🔐 Step 2: Credential & Server Setup

### SonarQube Setup
1. **Generate Token**: In SonarQube, go to `My Account > Security` and generate a **User Token**.
2. **Add to Jenkins**: Go to `Manage Jenkins > Credentials`. Add **Secret Text** with ID `sonar-token`.
3. **Configure System**: In `Manage Jenkins > System`, add a **SonarQube Server**:
    * **Name**: `SonarQube`
    * **Server URL**: `http://<your-ip>:9000`
    * **Token**: Select `sonar-token`.
4. **Webhook**: In SonarQube (`Administration > Configuration > Webhooks`), create a webhook pointing to: `http://<JENKINS_URL>/sonarqube-webhook/`.

### Aikido Setup
1. **Generate API Key**: Log in to Aikido, go to **Integrations > CI**, and generate an **API Key**.
2. **Add to Jenkins**: In Jenkins Credentials, add **Secret Text** with ID `aikido-token`.
3. **Install CLI**: On your Jenkins node, run: `npm install --global @aikidosec/ci-api-client`.

---

## 🏗 Step 3: Jenkins Job Configuration
1. Click **New Item** > **Pipeline** > Name it `simple-java-app`.
2. In the **Pipeline** section, set:
    * **Definition**: `Pipeline script from SCM`
    * **SCM**: `Git`
    * **Repository URL**: `https://github.com`
    * **Branch**: `*/POC`
    * **Script Path**: `jenkins/Jenkinsfile` <-- *Note the sub-folder path*
3. Click **Save** and **Build Now**.

---

## 📁 Repository Structure
* `src/`: Application source code and JUnit tests.
* `jenkins/Jenkinsfile`: The automated pipeline definition.
* `jenkins/scripts/deliver.sh`: Post-build delivery script (ensure `chmod +x` is applied).
* `pom.xml`: Maven config with **JaCoCo** for SonarQube coverage and **SonarScanner**.

---

## 🚦 Pipeline Stages
1. **Build**: Compiles code and skips tests for speed.
2. **Test**: Runs JUnit tests and generates coverage reports via JaCoCo.
3. **Security Scan (Aikido)**: Scans for SCA, SAST, and Secrets. Fails build on critical risks.
4. **SonarQube Analysis**: Performs static code analysis and uploads results.
5. **Quality Gate**: Pauses until SonarQube confirms code meets quality standards.
6. **Deliver**: Executes the final deployment/delivery script.
