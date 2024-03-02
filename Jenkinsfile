pipeline {
    agent any
    environment {
        registry = "iamsauravsingh/python-app"
        registryCredential = 'dockerhub'
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'project-1', url: 'https://github.com/iamsauravsingh7/kh-project.git'
            }
        }
        
        stage('Build docker image') {
            steps {
                script {
                  dockerImage = docker.build registry + ":v$BUILD_NUMBER"
              }
            }
        }
        stage('Upload Image'){
          steps{
            script {
              docker.withRegistry('', registryCredential) {
                dockerImage.push("v$BUILD_NUMBER")
                dockerImage.push('latest')
              }
            }
          }
        }
        stage('Remove Unused docker image') {
          steps{
            sh "docker rmi $registry:v$BUILD_NUMBER"
          }
        }
        stage('Deploying container to Kubernetes') {
           steps {
                def serviceExists = sh(
                    scripts "kubectl get service python-app -n default"
                    returnStatus: true
                ) ==0
               if (serviceExixts){
                   sh "helm install project-1 python-project --set appimage=${registry}:v${BUILD_NUMBER}"
               } else {
                    sh "helm install project-1 python-project --set appimage=${registry}:v${BUILD_NUMBER}"
            }
        }      
    }
}
