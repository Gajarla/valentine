pipeline {
    agent any

    environment {
        REPO = 'https://github.com/gajarla/valentine.git'
        CREDS = 'github-creds'
        BRANCH = 'main'
    }

    stages {

        stage('Clone') {
            steps {
                git branch: "${BRANCH}", credentialsId: "${CREDS}", url: "${REPO}"
            }
        }

        stage('Simulate Deployment') {
            steps {
                sh '''
                echo "Deployment triggered on $(date)" > deploy.txt
                '''
            }
        }

        stage('Push Changes') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${CREDS}", usernameVariable: 'USER', passwordVariable: 'TOKEN')]) {
                    sh '''
                    git config user.name "jenkins"
                    git config user.email "jenkins@local"

                    git add .
                    git commit -m "Jenkins Auto Deploy $(date)" || echo "No changes"

                    git push https://${USER}:${TOKEN}@github.com/gajarla/valentine.git main
                    '''
                }
            }
        }
    }
}
