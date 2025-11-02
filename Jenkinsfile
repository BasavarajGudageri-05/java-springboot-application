pipeline{
    agent any
    tools {
        jdk 'java-17'
        maven 'maven'

    }
    environment{
        IMAGE_NAME= "babugudageri/spring-boot:{GIT_COMMIT}"
    }
    stages{
        stage('git clone'){
            steps{
                git branch: 'main', url: 'https://github.com/BasavarajGudageri-05/java-springboot-application.git'
            }
            
        }
        stage('maven compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage ('maven build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('build image'){
            steps{
                echo 'building docker image'
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }
        stage('docker login'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerID', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
                     sh '''
                            echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USER" --password-stdin
                            docker push $IMAGE_NAME
                     '''

                    
}
   
                

            
            }
        }
        stage('Deploy in k8s'){
            steps{
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8sID', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
    
}               
                sh """
                  echo ' cretaing name space'
                  kubectl apply -f namespace.yaml
                  echo 'applying deployment and service'
                  kubectl apply -f Deployment.yaml
                """
               
            }
        }

    }
}
