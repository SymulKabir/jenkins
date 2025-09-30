### Jenkins CI/CD Setup with GitHub

#### 1. Install Required Plugins in Jenkins
- Go to **Manage Jenkins → Plugins → Available plugins**
- Install the following:
  - **Git Plugin**
  - **Pipeline Plugin**
  - **GitHub Branch Source Plugin** *(optional, for GitHub repo handling)*

---

#### 2. Create a Jenkins Pipeline Job
- Go to **Jenkins Dashboard → New Item → Pipeline**
- Give it a name (e.g., `private-repo-pipeline`)
- Select **Pipeline** and click **OK**

---


#### 4. Create a Jenkinsfile in GitHub Repository
- In your repository, create a file named **Jenkinsfile** with your pipeline steps.  
- Example: [View Jenkinsfile](Jenkinsfile)  

```bash
pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *') // poll every 2 minutes
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    credentialsId: 'github-access-token',
                    url: 'https://github.com/SymulKabir/symulkabir-next.js.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }


        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
}

```
---

#### 5. Configure GitHub Repository

Inside the job:

- Go to **Build Triggers** → Select **GitHub hook trigger for GITScm polling**
- In **Pipeline** section → Choose **Pipeline script from SCM**
- Configure the following:
  - **SCM**: Git  
    - **Repository URL**: `https://github.com/your-username/your-repo.git`  
    - **Branches to build**: `*/master`
  - **Script Path**: `Jenkinsfile` *(enter your pipeline filename here)*

---

#### (Optional) Configure Private GitHub Repository Access
- **Generate a GitHub Personal Access Token (PAT):**
  - Go to **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)**
  - Generate a new token with the following scopes:
    - **repo** (for private repository access)
    - **admin:repo_hook** *(optional, if you want Jenkins to manage webhooks automatically)*
  - Copy the token safely and store it in Jenkins credentials.

- **Add Credentials in Jenkins:**
  - Go to **Jenkins → Manage Jenkins → Credentials → System → Global credentials → Add Credentials**
  - Select **Username with password**
    - **Username:** your GitHub username  
    - **Password:** the PAT (personal access token) you generated  
  - Give it an ID (e.g., `github-access-token`)  

---

#### 6. Test the Pipeline
- Push a change to your repository:
```bash
git add .
git commit -m "test pipeline with polling"
git push origin master
```
Wait for the next poll cycle (`every 2 minutes`).  
If a change is detected, Jenkins will automatically **checkout, build, test, and deploy**.
 

