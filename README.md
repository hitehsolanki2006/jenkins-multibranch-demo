# 🚀 Jenkins Multibranch Pipeline Demo

A hands-on demo project to understand and practice **Jenkins Multibranch Pipeline** with branch-based CI/CD strategy. Each branch represents a real-world environment and has its own `Jenkinsfile` tailored to that branch's purpose.

---

## 📌 What This Project Demonstrates

- How Jenkins automatically detects and builds **multiple branches** from a single GitHub repository
- How each branch can have its **own pipeline logic** using separate `Jenkinsfile`s
- A practical **branch strategy** mapping to real deployment environments
- End-to-end connection between **GitHub → Jenkins Multibranch Pipeline**

---

## 🏗️ Branch Strategy

| Branch | Purpose | Environment |
|---|---|---|
| `main` | Stable production-ready code | PRODUCTION |
| `dev` | Integration & development testing | DEVELOPMENT |
| `feature/login` | Login feature development | FEATURE |
| `feature/payment` | Payment feature development | FEATURE |
| `hotfix/issue-12` | Critical bug fix | HOTFIX |

> 💡 Each branch has its own `Jenkinsfile` with pipeline stages customized for that branch's role.

---

## ⚙️ Prerequisites

Make sure the following are installed and ready before starting:

- ✅ **Jenkins** (v2.387+ recommended) — [Download Jenkins](https://www.jenkins.io/download/)
- ✅ **Java JDK 11 or 17** (required to run Jenkins)
- ✅ **Git** installed on the Jenkins server
- ✅ **GitHub account** with access to this repository
- ✅ **Jenkins Plugins** (install from Jenkins Plugin Manager):
  - `Git Plugin`
  - `Pipeline Plugin`
  - `Multibranch Pipeline Plugin`
  - `GitHub Branch Source Plugin`

---

## 🔌 Jenkins Plugin Installation

1. Open Jenkins → Go to **Manage Jenkins** → **Manage Plugins**
2. Click the **Available** tab
3. Search and install the following:
   - `Git Plugin`
   - `Pipeline`
   - `Multibranch Pipeline`
   - `GitHub Branch Source`
4. Click **Install without restart** (or restart if prompted)

---

## 🔗 Step 1 — Fork or Clone This Repository

```bash
git clone https://github.com/h-innoveda/jenkins-multibranch-demo.git
cd jenkins-multibranch-demo
```

Or **fork** it to your own GitHub account so you have full control.

---

## 🔐 Step 2 — Add GitHub Credentials in Jenkins(Optional)

Jenkins needs credentials to access your GitHub repository.

1. Go to **Jenkins Dashboard** → **Manage Jenkins** → **Credentials**
2. Click **System** → **Global credentials (unrestricted)**
3. Click **Add Credentials**
4. Fill in the form:
   - **Kind:** `Username with password`
   - **Username:** your GitHub username
   - **Password:** your GitHub **Personal Access Token** (PAT)
   - **ID:** `github-credentials` *(remember this ID)*
   - **Description:** `GitHub Access`
5. Click **Save**

> 💡 To generate a GitHub PAT: GitHub → Settings → Developer Settings → Personal Access Tokens → Generate new token → select `repo` scope.

---

## 🏗️ Step 3 — Create a Multibranch Pipeline Job in Jenkins

1. Go to **Jenkins Dashboard**
2. Click **New Item**
3. Enter a name: `jenkins-multibranch-demo`
4. Select **Multibranch Pipeline**
5. Click **OK**

---

## 🔗 Step 4 — Connect GitHub Repository to Jenkins

Inside the Multibranch Pipeline job configuration:

1. Under **Branch Sources**, click **Add source** → Select **GitHub** (or **Git**)
2. Fill in:
   - **Credentials:** select `github-credentials` (created in Step 2)
   - **Repository HTTPS URL:**
     ```
     https://github.com/h-innoveda/jenkins-multibranch-demo.git
     ```
3. Under **Behaviours**, keep defaults (Discover branches)
4. Under **Build Configuration**:
   - Mode: `by Jenkinsfile`
   - Script Path: `Jenkinsfile`
5. Click **Save**

---

## 🔍 Step 5 — Scan Repository & Auto-Detect Branches

1. After saving, Jenkins will automatically run a **Scan Repository**
2. It will discover all branches that contain a `Jenkinsfile`:
   - `main`
   - `dev`
   - `feature/login`
   - `feature/payment`
   - `hotfix/issue-12`
3. Jenkins will create a **separate pipeline** for each branch automatically
4. Each pipeline will trigger its first build

> 💡 You can manually trigger a scan anytime: Job → **Scan Repository Now**

---

## ▶️ Step 6 — Trigger & View Pipeline Runs

1. Go to your Multibranch Pipeline job
2. You will see all **5 branches listed** as separate jobs
3. Click on any branch (e.g., `main`) to see its pipeline
4. Click **Build Now** to trigger a manual build
5. Click on the build number → **Console Output** to see the echo messages

---

## 📁 Jenkinsfile Structure

Each branch contains a `Jenkinsfile` with 4 stages:

```groovy
pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo "Branch : <BRANCH_NAME>"
                echo "Task   : <TASK_DESCRIPTION>"
                echo "Status : Running Pipeline..."
            }
        }
        stage('Build') {
            steps {
                echo "Building <BRANCH> branch code..."
            }
        }
        stage('Test') {
            steps {
                echo "Running tests on <BRANCH> branch..."
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying to <ENVIRONMENT> environment..."
            }
        }
    }
    post {
        success {
            echo "<BRANCH> pipeline completed successfully!"
        }
    }
}
```

| Stage | Description |
|---|---|
| `Hello` | Prints branch info and task details |
| `Build` | Simulates build step for the branch |
| `Test` | Simulates test execution |
| `Deploy` | Simulates deployment to target environment |
| `post > success` | Confirmation message on successful run |

---

## 🌐 Optional — GitHub Webhook Setup (Auto Trigger on Push)

To make Jenkins **automatically trigger** a build when you push code:

1. Go to your GitHub repo → **Settings** → **Webhooks** → **Add webhook**
2. Fill in:
   - **Payload URL:** `http://<YOUR_JENKINS_URL>/github-webhook/`
   - **Content type:** `application/json`
   - **Trigger:** `Just the push event`
3. Click **Add webhook**
4. In Jenkins → Job → **Configure** → enable **"Scan by webhook"** or **"GitHub hook trigger for GITScm polling"**

> 💡 Jenkins must be publicly accessible (or use tools like **ngrok** for local Jenkins).

---

## 📂 Repository Structure

```
jenkins-multibranch-demo/
│
├── Jenkinsfile          # Pipeline definition for current branch
└── README.md            # Project documentation
```

> Each branch has its own `Jenkinsfile` with branch-specific echo messages.

---

## 🙋 Author

**h-innoveda**
GitHub: [@h-innoveda](https://github.com/h-innoveda)

---

## 📄 License

This project is open source and available for learning purposes.
