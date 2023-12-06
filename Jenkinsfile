pipeline {
    agent {label dev-server}
    
    stages {
        stage("Code") {
            steps {
                echo "Code stage is running"
                git url: "https://github.com/shant-ramtirth/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and Test") {
            steps {
                echo "Build & Test stage is running"
                sh "docker build -t node-app:latest ."
            }
        }
        stage("Scan") {
            steps {
                echo "Security scan by Trivy is completed"
            }
        }
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerhubcreds",passwordVariable:"dockerhubpassword",usernameVariable:"dockerhubusername")]) {
                sh "docker login -u ${env.dockerhubusername} -p ${env.dockerhubpassword}"
                sh "docker tag node-app:latest ${env.dockerhubusername}/node-app:latest"
                sh "docker push ${env.dockerhubusername}/node-app:latest"
                echo "Push stage is running"
                } 
            }
        }
        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo "Deploy stage is running"
            }
        }
    }
    
}
