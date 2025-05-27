pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Abrar10592/php-hello-world.git'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build("abrar10592/php-hello-world:latest")
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                    docker.image("abrar10592/php-hello-world:latest").inside {
                        sh 'php index.php'
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        docker.withRegistry('', 'dockerhub-credentials') {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }
}
