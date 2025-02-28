pipeline {
    agent any
    environment {
        PATH = "/opt/homebrew/Cellar/maven/3.9.9/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
        MAVEN_HOME = "/opt/homebrew/Cellar/maven/3.9.9/libexec"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: 'github-pat',
                url: 'https://github.com/kunalkagarwal/spe-mini-calculator.git',
                branch: 'main'
            }
        }
        stage('Debug Maven Path') {
            steps {
                sh 'echo $PATH'
                sh 'which mvn'
                sh 'mvn -version'
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
