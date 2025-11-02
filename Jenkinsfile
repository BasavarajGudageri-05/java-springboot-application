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
       stage{
           steps{
               
                 echo "Deploying to Kubernetes"
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8sID', namespace: 'spring-boot', restrictKubeConfigAccess: false, serverUrl: '')  {
                    sh """ 
                        
                       echo 'applying namespace'
                         kubectl appy -f namespace.yaml
                         echo '📦 Applying deployment'
                         kubectl apply -f Deployment.yaml
                         }
                         }

                         

            
            }
        }
        stage('Deploy in k8s'){
            steps{
          
               
            }
        }

    }
}
