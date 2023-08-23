pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/akshat5302/bmc-k8s-deployment.git", branch: "main"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t bmc-react-app"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub-credentials",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag bmc-react-app ${env.dockerHubUser}/bmc-react-app:latest"
                    sh "docker push ${env.dockerHubUser}/bmc-react-app:latest" 
                }
            }
        }
        // stage("Deploy"){
        //     steps{
        //         sh "docker-compose down && docker-compose up -d"
        //     }
        // }
        stage("Deployusingk8s "){
            steps{
                sh "kubectl apply -f deployment.yaml"
                sh "kubectl apply -f service.yaml"
            }
        }
    }
}