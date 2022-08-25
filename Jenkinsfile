pipeline {
    agent any
    tools{
        maven 'MAVEN3'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/naren-21/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t dnkumar/devops-sitarama21 .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'docker-cred2', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u vuddanti -p ${dockerhubpwd}'

}
                    sh 'docker tag abhinav/devops-integration vuddanti/devops' 
                    sh 'docker push abhinav/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
