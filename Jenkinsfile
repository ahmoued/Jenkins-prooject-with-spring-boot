pipeline {
    agent any

    environment {
        IMAGE_NAME = 'ahmedoued/mon-app-springboot'
        TAG = "build-${BUILD_NUMBER}"
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git branch: 'main', url: 'https://github.com/ahmoued/Jenkins-prooject-with-spring-boot'
            }
        }

        stage('Build avec Maven') {
            steps {
                sh './mvnw clean install'
            }
        }

        stage('Construire l’image Docker') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Tests') {
            steps {
                sh './mvnw test'
            }
        }

        stage('Push vers Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'ahmoued', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag $IMAGE_NAME:$TAG $IMAGE_NAME:latest
                        docker push $IMAGE_NAME:$TAG
                        docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }
    }
}
