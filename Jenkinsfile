pipeline {
    agent { label "NODE-DEV"}
    
    stages{
        stage('Build'){
            steps {
                echo "Building the Image"
                sh "sudo docker build -t gopalgtm001/backend-workshop:latest /home/ubuntu/workshop/application-code/app-tier"
                sh "sudo docker build -t gopalgtm001/frontend-workshop:latest /home/ubuntu/workshop/application-code/web-tier"
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"docker",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/backend-workshop:latest"
                sh "docker push ${env.dockerHubUser}/frontend-workshop:latest"
                echo "hello"
                }
            }
        }
        stage('Deploy'){
            steps {
                echo "Deploying the application"
                sh "sudo docker run -d -p 4000:4000 gopalgtm001/backend-workshop:latest"
                sh "sudo docker run -d -p 3000:3000 gopalgtm001/frontend-workshop:latest"
            }
        }
    }
}
