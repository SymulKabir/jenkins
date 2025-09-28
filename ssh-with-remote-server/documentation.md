### Install the SSH Agent Plugin in Jenkins

- Go to your Jenkins dashboard.
- Navigate to **Manage Jenkins → Manage Plugins → Available**.
- Search for **SSH Agent**.
- Check it and click **Install without restart**.

### Generate an SSH Key (if not already done)
Login to your remote server

#### 1. Regenerate (or convert) a proper private key on your remote server:
```bash
ssh-keygen -t rsa -b 4096 -m PEM -f ~/.ssh/jenkins_key
```
This gives you:

- `~/.ssh/jenkins_key` (private key – paste this into Jenkins credentials)
- `~/.ssh/jenkins_key.pub` (public key – put this into `~/.ssh/authorized_keys` on your server)

### Add the private key to Jenkins:
- Go to **Manage Jenkins → Credentials → (Global)**.
- Add new credential → **"SSH Username with private key"**.
- Username: `root` (or the user you use).
- Login your remote server and run `cat ~/.ssh/jenkins_key` and copy the whole output.
- Private Key: paste the entire jenkins_key file (starts with -----BEGIN RSA PRIVATE KEY-----).
- Save with ID: micple-server.

### Create a New Pipeline Job
- Go to `Jenkins Dashboard`.
- Click `New Item`.
- Enter a `name` for your job.
- Select `Pipeline`.
- Click `OK`.


### Configure the Pipeline
Scroll down to **Pipeline** section
Then **Definition** -> **Pipeline script**

Write a Simple Pipeline Script in **Script** section:-
[View pipeline file](https://github.com/SymulKabir/jenkins/blob/main/ssh-with-remote-server/pipeline)
```bash
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                sshagent(['micple-server']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no root@micple.com "
                            mkdir -p /var/micple.com/jenkisTestFolder
                        "
                    '''
                }
            }
        }
    }
}

```
