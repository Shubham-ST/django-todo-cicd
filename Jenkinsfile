pipeline {
    agent { label "dev-agent"}
    stages{
        stage("Code") {
            steps {
                echo "code cloning"
                git url: "https://github.com/Shubham-ST/django-todo-cicd.git" , branch: "develop"
            }
        }
        stage("Build And Test") {
            steps {
                echo "Code building and Testing"
                sh "docker build . -t todo-app-new"
            }
        }
        stage("Login and Push image to Dockergub") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubUser")]) {
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubpass}"
                sh "docker tag todo-app-new ${env.dockerhubUser}/todo-app-new:latest"
                sh "docker push ${env.dockerhubUser}/todo-app-new:latest"
                echo "Image pushed on dockerhub"
                }
                
            }
        }
        stage("Deploy") {
            steps {
                echo "Code Deployed..."
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    
}

