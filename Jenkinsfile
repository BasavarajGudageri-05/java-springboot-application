pipeline{
    agent any
    tools {
        jdk 'java-17'
        maven 'maven'

    }
    environment{
        IMAGE_NAME= "babugudageri/spring-boot"
    }
    stages{
        stage('git clone'){
            git branch: 'main', url: 'https://github.com/BasavarajGudageri-05/java-springboot-application.git'
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
                sh 'docker build -t $IMAGENAME:latest .'
            }
        }
        stage('docker login'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerID', passwordVariable: 'DOCKER_PASSOWRD', usernameVariable: 'DOCKER_USER')]) {
                    sh """
                echo "$DOCKER_PASSOWRD" | docker login -u "DOCKER_USER" --password -stdin
                docker push $IMAGE_NAME:latest
                 """
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
