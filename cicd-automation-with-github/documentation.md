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


### 4. Create a `Jenkinsfile` in GitHub Repo
In your repository, create a file named `Jenkinsfile` with pipeline steps.
Example:

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




