pipeline {
    agent any  // Run on any available agent

   environment {
    DOCKER_HUB_USER = credentials('docker-hub-credentials')
}

    stages {
        stage('Checkout Code') {
    steps {
        git credentialsId: 'github-credentials', url: 'https://github.com/pateldev11/LAB-DEVOPS.git', branch: 'main'
    }
}


        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    sh "echo $DOCKER_HUB_PASS | docker login -u $DOCKER_HUB_USER --password-stdin"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                sh "docker push $DOCKER_IMAGE"
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker run -d -p 8080:8080 $DOCKER_IMAGE"
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check errors in Jenkins logs.'
        }
    }
}
