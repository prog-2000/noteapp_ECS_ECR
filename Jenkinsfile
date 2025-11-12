pipeline {
    agent any 
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/prog-2000/noteapp-ci-cd.git", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building the image"
                bat 'docker build -t my-note-app .'
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    bat 'docker tag my-note-app %dockerHubUser%/my-note-app:latest'
                    bat 'docker login -u %dockerHubUser% -p %dockerHubPass%'
                    bat 'docker push %dockerHubUser%/my-note-app:latest'
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying the container"
                bat 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
