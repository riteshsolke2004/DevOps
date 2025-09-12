## 2) Configure Global Tools & Credentials (after first login)
### 2.1 Global Tools (Manage Jenkins → Global Tool Configuration)
- **JDK installations** (if you rely on system Java, can leave blank).
- **Git** (path to git binary, or Jenkins can auto-install).
- **Maven** (Add Maven installation name e.g., `Maven3.8` and automatic install).
- **NodeJS** (if building JS apps, add Node installer plugin first or use docker agent).
- **Docker** — configure Docker host if you intend to use Docker agents or build images on server (usually better to use docker agents or run docker-in-docker agents).

### 2.2 Add Credentials (Manage Jenkins → Credentials → System → Global)
Add these commonly used types:
- **SSH Username with private key** — for server deploys or agent setups.
- **Username with password** — legacy APIs, docker hub, artifact repos.
- **Secret Text** — tokens (GitHub token, DockerHub token, etc.).
- **AWS Credentials (access key/secret)** if using plugins for AWS.

Use descriptive IDs (e.g., `github-token`, `deploy-ssh-key`, `docker-hub-creds`).

---
## 3) Jenkins Installation details & housekeeping
- Jenkins home: `/var/lib/jenkins` — backup this directory for config, jobs, credentials (encrypted), plugins, nodes.
- Plugins folder: `/var/lib/jenkins/plugins`.
- To install plugins from CLI or update: `Manage Plugins` or use `jenkins-cli.jar` for automation.
- Keep Jenkins and plugins up-to-date; test upgrades on a separate instance first.

---
## 4) Jenkins UI / Dashboard / Jobs — Practical Steps
### 4.1 Basic UI sections
- **New Item**: Create Freestyle Job / Pipeline / Multibranch Pipeline / Folder.
- **Build History**: past runs & statuses.
- **Manage Jenkins**: global config, plugins, security.
- **Credentials**: central credential store.
- **Nodes**: connected agents.

### 4.2 Create a Freestyle job (step-by-step)
1. New Item → Name → choose **Freestyle project** → OK.
2. **Source Code Management** → Git → Put repo URL and choose credentials (if private).
3. **Build Triggers** → Check *GitHub hook trigger for GITScm polling* or *Poll SCM*.
4. **Build Environment** → add any env injection/credentials binding.
5. **Build Steps** → *Execute shell* (for Linux agents):
```bash
# Example: Maven build
mvn -B -DskipTests clean package

# Node app example
npm install
npm test
```
6. **Post-build Actions** → Archive artifacts `target/*.jar`, JUnit test results, publish to Nexus/Artifactory, send Slack/Email notifications.

### 4.3 Pros/Cons of Freestyle
- Good for simple tasks and legacy builds. Not ideal for complex pipelines or code-as-config. Prefer "Pipeline" for reproducible, versionable build logic.

---
