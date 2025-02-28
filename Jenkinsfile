pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'gitHub-pat',
                url: 'https://github.com/kunalkagarwal/spe-mini-calculator.git'
            }
        }
        stage('Build Code') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test Code') {
            steps {
                sh 'mvn test'
            }
        }
    }
    post {
        success {
            echo 'Build and tests passed!'
        }
        failure {
            echo 'Build failed! Check the logs.'
        }
    }
}