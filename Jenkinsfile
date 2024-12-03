pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                echo 'Cloning the code'
                git url: "https://github.com/Haris127/django-notes-app.git", branch: "main"
            }
        }
        stage('Build') {
            steps {
                echo 'Building the image'
                sh "docker build -t my-notes-app ."
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing the image to Docker Hub'
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerhubPass", usernameVariable:"dockerhubUser")]){
                sh "docker tag my-notes-app ${env.dockerhubUser}/my-notes-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p '${env.dockerhubPass}'"
                sh "docker push ${env.dockerhubUser}/my-notes-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the containers'
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
