#### 1. Install Required Plugins in Jenkins

- Go to **Manage Jenkins → Plugins → Available plugins**
- **Install:**
-- GitHub Integration Plugin
- Install:
    - GitHub Integration Plugin
     - Pipeline Plugin
     - Git Plugin
     - GitHub Branch Source Plugin

#### 2. Create a Jenkins Pipeline Job
- Go to **Jenkins Dashboard → New Item → Pipeline**
- Give it a name (e.g., `github-master-pipeline`)
- Select **Pipeline** and click **OK**

### 3. Configure GitHub Repository

Inside the job:

- Go to **Build Triggers** → Select **GitHub hook trigger for GITScm polling**
- In **Pipeline** section → Choose **Pipeline script from SCM**
- Configure the following:
  - **SCM**: Git  
    - **Repository URL**: `https://github.com/your-username/your-repo.git`  
    - **Branches to build**: `*/master`
  - **Script Path**: `Jenkinsfile` *(enter your pipeline filename here)*

#### (Optional) if repository is private GitHub repository
- #### Generate a GitHub Personal Access Token (PAT):
    - Go to **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)**
    - Generate a new token with the following scopes:
    - `repo` (for private repository access)
    - `admin:repo_hook` (to let Jenkins set webhooks — optional if you configure webhooks manually)
    - Copy the token safely and store it in Jenkins credentials.
- #### Add Credentials in Jenkins

    - Go to **Jenkins → Manage Jenkins → Credentials → System → Global credentials → Add Credentials**
    - Select **Username with password**:
    - **Username**: your GitHub username  
    - **Password**: the PAT (personal access token) you generated
    - Give it an **ID** (e.g., `github-creds`)


### 4. Create a `Jenkinsfile` in GitHub Repo
In your repository, create a file named `Jenkinsfile` with pipeline steps.
Example:

[View pipeline file](Jenkinsfile)

```bash
pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', 
                credentialsId: 'github_access_token',
                url: 'https://github.com/SymulKabir/somachartv-express.js.git'
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

### 5. Set Up Webhook in GitHub

- Go to your GitHub repository → **Settings** → **Webhooks** → **Add webhook**
- Configure the following:
   - **Payload URL**: `http://<your-jenkins-server>:8080/github-webhook/`
   - **Content type**: `application/json`
   - Which events would you like to trigger this webhook → **Just the push event**
- Click **Save**

### 6. Test  

- Push a change to the `master` branch:  

   ```bash
   git add .
   git commit -m "test pipeline"
   git push origin master
   
   ```
- Jenkins will trigger the pipeline automatically.




