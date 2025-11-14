@Library("SharedLib") _

pipeline {
    agent {label 'kali'}

    stages {
        stage('Hello'){
            steps {
                script{
                    hello()
                }
            }
        }
        stage('Code') {
            steps {
                clone("https://github.com/sagarmemane135/django-notes-app.git","main")
                // echo "This is cloning the code"
                // git url: "https://github.com/sagarmemane135/django-notes-app.git", branch: "main"
                // echo "Code cloned successfully !"
            }
        }
        stage('Build') {
            steps {
                echo "This is building the code"
                sh "docker build -t notes-app:latest ."
                echo "Docker image Build succeeded!"
            }
        }
        stage('Test') {
            steps {
                echo "This is testing the code"
                sh "trivy image --severity HIGH,CRITICAL notes-app:latest"
                echo "Docker Image Scan Complete"
            }
        }
        stage('Push to DockerHub') {
            steps {
                echo "This is pushing the image"
                withCredentials([usernamePassword('credentialsId':'dockerhubCred',usernameVariable:'dockerhubUser',passwordVariable:'dockerhubPass')]){
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    sh "docker image tag notes-app:latest ${env.dockerhubUser}/notes-app:latest"
                    sh "docker push ${env.dockerhubUser}/notes-app:latest"
                }
                echo "Docker Image Scan Complete"
            }
        }
        stage('Deploy') {
            steps {
                echo "This is deploying the code"
                sh "docker compose up -d"
                echo "Container Started !!"
            }
        }
    }
}
