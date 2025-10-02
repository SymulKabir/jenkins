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
