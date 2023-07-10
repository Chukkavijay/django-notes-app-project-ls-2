pipeline{
    agent any
    stages{
        stage("Code Checkout"){
            steps{
                echo "code checkout done"
                git url:"https://github.com/Chukkavijay/django-notes-app-project-ls-2.git", branch: "main"
            }
        }
        stage("Docker Image Build"){
            steps{
                echo "Building Docker Image"
                sh "docker image build -t my-note-app ."
            }
        }
        stage("Docker Image Push"){
            steps{
                echo "Pushing Docker Image into DockerHub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker image tag my-note-app ${env.dockerhubUser}/my-note-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker image push ${env.dockerhubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploying apllication"){
            steps{
                echo "deploying docker image to Docker Container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
