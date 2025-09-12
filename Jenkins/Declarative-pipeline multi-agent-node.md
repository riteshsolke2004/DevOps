## 5) Declarative Pipeline (Jenkinsfile) — Real example + explanation
**Why use Jenkinsfile?** Pipelines as code: versioned with the repo, easier review, more reproducible.

**Example `Jenkinsfile` (declarative, multi-stage, Docker build & deploy):**
```groovy
pipeline {
  agent any
  environment {
    MVN = tool name: 'Maven3.8', type: 'maven'
    DOCKER_IMAGE = "myorg/myapp:${env.BUILD_NUMBER}"
  }
  options {
    timestamps()
    buildDiscarder(logRotator(numToKeepStr: '10'))
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh "${MVN}/bin/mvn -B -DskipTests clean package"
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
    stage('Unit Test') {
      steps {
        sh "${MVN}/bin/mvn test"
        junit 'target/surefire-reports/*.xml'
      }
    }
    stage('Docker Build & Push') {
      when { branch 'main' }
      agent { docker { image 'docker:24-cli' args '-v /var/run/docker.sock:/var/run/docker.sock' } }
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
          sh "docker build -t ${DOCKER_IMAGE} ."
          sh "docker push ${DOCKER_IMAGE}"
        }
      }
    }
    stage('Deploy') {
      when { branch 'main' }
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'prod-ssh', keyFileVariable: 'SSHKEY')]) {
          sh 'ssh -i $SSHKEY ubuntu@prod-server "docker pull ${DOCKER_IMAGE} && docker run -d --rm --name myapp ${DOCKER_IMAGE}"'
        }
      }
    }
  }
  post {
    success { echo "Build succeeded: ${env.BUILD_URL}" }
    failure { echo "Build failed - notify team" }
  }
}
```
**Notes on the example:**
- `agent any` runs on master or any available agent. You can target specific nodes via `agent { label 'linux && docker' }`.
- `tool` is the name from Global Tool Configuration.
- `withCredentials` injects secure secrets for runtime use (never echo secrets!).
- Use `archiveArtifacts`, `junit` to preserve results and show in UI.

---
## 6) Multi-Node / Agents — Setup & Usage
**Two primary ways to connect agents: SSH (Controller launches) and JNLP (agent connects to controller).**

### 6.1 Prepare agent machine(s)
```bash
# On agent machine
sudo apt update && sudo apt install -y openjdk-11-jdk git
# create a jenkins user (optional)
sudo useradd -m -s /bin/bash jenkins
sudo passwd jenkins
sudo usermod -aG docker jenkins   # if docker builds required and docker installed
```
**Note:** Java version on agent must be compatible with Jenkins agent requirements.

### 6.2 SSH agent (controller connects to agent)
1. In Jenkins: **Manage Jenkins → Manage Nodes and Clouds → New Node**.
2. Fill name, choose "Permanent Agent".
3. Set Remote root directory (e.g., `/home/jenkins/agent`), labels (`linux docker`), usage.
4. Launch method: **Launch agents via SSH**. Add host, credentials (SSH username + private key). Test connection.
5. Jenkins will copy the agent jar and start agent process on remote host (requires proper permissions).

### 6.3 JNLP agent (agent connects to controller) — recommended for NAT/behind firewall
1. In Manage Jenkins → Configure Global Security → set **TCP port for JNLP agents** to a fixed value (e.g., 50000).
2. Create new node in Jenkins (as above) but choose launch method **Launch agent by connecting it to the controller**.
3. On the node page, you'll find the **agent.jar** download and the command line JNLP string:
```bash
# Example (run on agent host)
wget http://<jenkins-host>:8080/jnlpJars/agent.jar -O agent.jar
java -jar agent.jar -jnlpUrl http://<jenkins-host>:8080/computer/<agent-name>/jenkins-agent.jnlp -secret <SECRET> -workDir /home/jenkins/agent
```
4. To run agent as a service, create a systemd unit:
```ini
# /etc/systemd/system/jenkins-agent.service
[Unit]
Description=Jenkins Agent
After=network.target

[Service]
User=jenkins
Restart=always
ExecStart=/usr/bin/java -jar /opt/jenkins/agent.jar -jnlpUrl http://<jenkins-host>:8080/computer/<agent>/jenkins-agent.jnlp -secret <SECRET> -workDir /home/jenkins/agent

[Install]
WantedBy=multi-user.target
```
Then `sudo systemctl daemon-reload && sudo systemctl enable --now jenkins-agent`.

### 6.4 Troubleshooting agents
- Ensure port 50000 and 8080 reachable or agent can reach controller.
- Check Java versions; agent jar may require Java 8+.
- File permissions for the agent workDir.
- For docker builds, ensure agent has docker CLI and access to docker socket (or use docker-in-docker containers in pipeline).
