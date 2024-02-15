pipeline {
    agent { label "NODE-DEV"}
    
    stages{
        stage('Build'){
            steps {
                echo "Building the Image"
                sh "sudo docker build -t gopalgtm001/backend-workshop:${env.BUILD_NUMBER} /home/ubuntu/workspace/sample/application-code/app-tier"
                sh "sudo docker build -t gopalgtm001/frontend-workshop:${env.BUILD_NUMBER} /home/ubuntu/workspace/sample/application-code/web-tier"
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"docker",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/backend-workshop:${env.BUILD_NUMBER}"
                sh "docker push ${env.dockerHubUser}/frontend-workshop:${env.BUILD_NUMBER}"
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def imagePath = "/home/ubuntu/workspace/sample/k8s"
                    sh "sed -i 's/image: gopalgtm001\\/backend-workshop:.*/image: gopalgtm001\\/backend-workshop:${env.BUILD_NUMBER}/g' ${imagePath}/backend-deployment.yml"
                    sh "sed -i 's/image: gopalgtm001\\/frontend-workshop:.*/image: gopalgtm001\\/frontend-workshop:${env.BUILD_NUMBER}/g' ${imagePath}/frontend-deployment.yml"
                    sh "kubectl apply -f ${imagepath}/backend-deployment.yml"
                    sh "kubectl apply -f ${imagepath}/frontend-deployment.yml"
                    sh "kubectl apply -f ${imagepath}/backend-svc.yml"
                    sh "kubectl apply -f ${imagepath}/frontend-svc.yml"
                }
            }
        }
    }
}