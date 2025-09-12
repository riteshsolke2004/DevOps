## 7) Multibranch Pipeline & GitHub Integration (webhook)
1. Create **Multibranch Pipeline** job → set **Branch Sources** → Add `Git` or `GitHub` → repo URL → credentials.
2. Configure **Scan Multibranch Pipeline Triggers** (e.g., Periodically or webhook-based).
3. On GitHub repo → Settings → Webhooks → Add webhook:
   - Payload URL: `http://<jenkins-host>:8080/github-webhook/` (or your Jenkins endpoint)
   - Content type: `application/json`
   - Secret: (optional)
   - Events: Choose *Pushes* and *Pull requests*.
4. Test push: pushing to `main` triggers Jenkins to scan and run Jenkinsfile found in repository root or branch root.

---
## 8) Shared Libraries — How to create & use
**Purpose:** Reuse common pipeline functions & steps across many Jenkinsfiles.

### 8.1 Repo layout for shared library
```
jenkins-shared-lib/
  vars/
    buildAndTest.groovy    # callable as buildAndTest()
  src/
    org/example/Utils.groovy  # class-based helpers
  resources/
    config/mytemplate.txt
```

**Example `vars/buildAndTest.groovy`:**
```groovy
def call(Map config = [:]) {
  stage('BuildShared') {
    sh "mvn -B -DskipTests clean package"
  }
  stage('TestShared') {
    sh "mvn test"
    junit 'target/surefire-reports/*.xml'
  }
}
```

### 8.2 Configure Jenkins to use the library
- Manage Jenkins → Configure System → Global Pipeline Libraries → Add
  - Name: `shared-lib`
  - Default version: `main`
  - Retrieval method: Modern SCM → Git → repo URL & credentials.

### 8.3 Use in a Jenkinsfile
```groovy
@Library('shared-lib@main') _
buildAndTest([foo: 'bar'])
```

---
## 9) User Management & Role-Based Access Control (RBAC)
### 9.1 Install plugin
- Install **Role-based Authorization Strategy** plugin (Manage Plugins → Available).
- Also consider **Folder-based Authorization** plugins and **Matrix-based Security** for other needs.

### 9.2 Configure security
1. Manage Jenkins → Configure Global Security → Authorization → select **Role-Based Strategy**.
2. Manage Jenkins → Manage and Assign Roles → Manage Roles: add global roles (admin, dev, view) and set permissions for each (read, build, configure, create, delete).
3. Manage and Assign Roles → Assign Roles: assign actual Jenkins user IDs to roles.

### 9.3 Create users
- Manage Jenkins → Security Realm → Jenkins' own user database → create users or integrate with LDAP/GitHub OAuth for enterprise SSO.

### 9.4 Folder-level roles
- Use **Folders** plugin + Project Roles to grant permissions to specific repositories/pipelines/folders only.

**Security tips:** follow least privilege, rotate credentials, use separate accounts for service integration.

---
## 10) Credentials Binding & Secrets usage in Pipelines
**Example: use GitHub token and SSH key in pipeline:**
```groovy
withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
  sh "curl -H 'Authorization: token $GITHUB_TOKEN' https://api.github.com/user"
}

withCredentials([sshUserPrivateKey(credentialsId: 'deploy-ssh', keyFileVariable: 'SSH_KEY')]) {
  sh 'scp -i $SSH_KEY target/app.jar ubuntu@prod:/opt/apps/'
}
```
**Rule:** never print secrets to logs. Use `maskPasswords` plugin or credentials-binding to hide secrets in console output.

---
## 11) Backups, Upgrades & Disaster Recovery
- Backup `JENKINS_HOME` (`/var/lib/jenkins`) regularly (configs, job definitions, plugin list).
- Use `thinBackup` plugin or simple `tar` + store on S3: `sudo tar czf jenkins-backup.tgz /var/lib/jenkins`.
- Test plugin upgrades in a staging Jenkins before production upgrades.
- Keep a list of installed plugins: `cat /var/lib/jenkins/plugins/*.jpi.pinned` (or use jenkins-cli to list).

---
## 12) Troubleshooting common errors & fixes
- **Port 8080 unreachable**: check AWS SG & ufw, verify `systemctl status jenkins`.
- **Agent offline**: check agent logs, Java version mismatch, firewall, wrong secret.
- **Plugin version errors**: rollback plugin from backup or reinstall compatible versions.
- **Permission denied for workspace**: ensure user running agent has RW access to work dir.
- **Docker builds failing**: ensure docker daemon is available and agent user has `docker` permission, or use docker-in-docker agent or Kubernetes plugin.

---
## 13) Best Practices & Workflow Tips (my opinion, short)
- Use **Jenkinsfile** (Declarative) stored in repo (no GUI steps). It's modern and reviewable.
- Use **Multibranch Pipelines** or **Organization Folders** for many repos — saves manual jobs.
- Prefer **ephemeral agents** (containers/Kubernetes) to keep builds isolated and reproducible.
- Put reusable logic in **Shared Libraries**, not giant Jenkinsfiles.
- **CI fast**: keep unit tests fast, run slow/expensive tests in separate stage or nightly builds.
- **Secure** everything: credentials, least privilege, and plugin hygiene.
- Use **artifact repositories** (Nexus/Artifactory) for artifacts — don't leave build artifacts on Jenkins nodes forever.

---
## 14) Handy Cheatsheet Commands
```bash
# Check Jenkins service
sudo systemctl status jenkins

# Initial password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

# Install Java (Ubuntu)
sudo apt install -y openjdk-11-jdk

# Download agent jar (from controller)
wget http://<jenkins>:8080/jnlpJars/agent.jar -O agent.jar

# Run a JNLP agent
java -jar agent.jar -jnlpUrl http://<jenkins>:8080/computer/<node>/jenkins-agent.jnlp -secret <SECRET> -workDir /home/jenkins/agent
```

---
## 15) Quick Example Repo structure for a typical app
```
my-app/
  Jenkinsfile
  pom.xml            # Maven project
  src/
  .github/           # webhooks / actions (if used)
```

---
## 16) Next steps you can ask me to do (I can add directly to your repo)
- Create a sample `Jenkinsfile` tuned to your app (Maven/Node/Python).
- Create a sample Shared Library repo with a `buildAndTest` var and usage example.
- Add a README with diagrams for Master-Agent and CI flow (I can generate ASCII diagrams or markdown-embedded images).
- Create step-by-step screenshots for each Jenkins UI action.

---
### Done — how to use this file
1. Save as `jenkins_deepdive.md` in your GitHub repo's docs folder.
2. Follow the “Jenkins Setup on VM” section to get Jenkins running.
3. Create a Multibranch pipeline pointing to your repo with `Jenkinsfile` and observe runs.
