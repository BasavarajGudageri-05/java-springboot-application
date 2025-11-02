pipeline {
    agent any

    tools {
        jdk 'java-17'
        maven 'maven'
    }

    environment {
        IMAGE_NAME = "babugudageri/spring-boot"
    }

    stages {

        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/BasavarajGudageri-05/java-springboot-application.git'
            }
        }

        stage('Maven Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🏗️ Building Docker image...'
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerID', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "🚀 Deploying to Kubernetes..."
                withKubeConfig(
                    credentialsId: 'k8sID',
                    namespace: 'spring-boot'
                ) {
                    sh '''
                      
                        
                        echo '📦 Applying deployment...'
                        kubectl apply -f Deployment.yaml
                        
                        echo '✅ Deployment complete!'
                    '''
                }
            }
        }
    }
}
