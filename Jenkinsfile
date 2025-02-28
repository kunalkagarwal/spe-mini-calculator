pipeline {
    agent any
    environment {
        PATH = "/opt/homebrew/Cellar/maven/3.9.9/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
        MAVEN_HOME = "/opt/homebrew/Cellar/maven/3.9.9/libexec"
        DOCKER_IMAGE_NAME = 'scientific-calculator'
            DOCKER_HUB_REPO = 'kunalkagarwal/scientific-calculator'

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

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE_NAME} ."
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-jenkins-pat', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    sh "docker tag ${DOCKER_IMAGE_NAME} ${DOCKER_HUB_REPO}:latest"
                    sh "docker push ${DOCKER_HUB_REPO}:latest"
                }
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
