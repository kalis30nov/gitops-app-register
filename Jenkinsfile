pipeline {
    agent {label: 'Jenkins-Agent'}
    environment {
        APP_NAME = "app-register-pipeline"
    }

    stages {
        stage ("Clean Workspace") {
            steps {
                cleanWs()
            }
        }

        stage ("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/kalis30nov/gitops-app-register'
            }
        }

        stage ("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "Kalimuthu"
                    git config --global user.email "kalimuthu30nov@yahoo.com"
                    git add deployment.yml
                    git commit -m "Deployment updated with ${APP_NAME}:${IMAGE_TAG}"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/kalis30nov/gitops-app-register main"
                }
            }
        }
    }
}