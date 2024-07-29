pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockercred')
        DOCKERHUB_REPO = 'utsavshah0305/day14-docker-jenkins'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Utsav0701/Day-14-jenkins-dockerfile.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def image = docker.build("java-app:${env.BUILD_ID}")
                    env.DOCKER_IMAGE = image.id
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image(env.DOCKER_IMAGE).push('latest')
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh 'docker run -d --name java-app -p 8080:8080 ' + env.DOCKERHUB_REPO + ':latest'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
