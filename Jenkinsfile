pipeline {
    agent any
    triggers { pollSCM('*/5 * * * *') // Vérifier toutes les 5 minutes
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dh_cred')

    }
    stages {
        stage('Checkout'){
            agent any
            steps {
                // Remove the existing directory, if it exists
                sh 'rm -rf projet_back'
                // Replace with the actual URL of your Git repository and the branch you want to check out
                sh 'git clone -b main https://github.com/azizz98/projet_back.git'
            }
        }
        stage('Init'){
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Build'){
            steps {
                sh 'ls'
                sh 'docker build -t aziztn/projet_back:$BUILD_ID projet_back/. '
            }
        }
        stage('Deliver'){
            steps {
                sh 'docker push aziztn/projet_back:$BUILD_ID'
            }
        }
        stage('Cleanup'){
            steps {
                sh 'docker rmi aziztn/projet_back:$BUILD_ID'
                sh 'docker logout'
            }
        }
    }
}
