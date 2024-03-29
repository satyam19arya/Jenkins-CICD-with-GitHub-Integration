pipeline {
    agent any 
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/satyam19arya/Jenkins-CICD-with-GitHub-Integration.git", branch: "master"
            }
        }
        
        stage("Build") {
            steps {
                echo "Building the image"
                // sh 'docker stop $(docker ps -a -q)'
                // sh 'docker rm $(docker ps -a -q)'
                // sh 'docker rmi $(docker images -q)'
                sh 'docker build -t node-app .'
            }
        }
        
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag node-app ${env.dockerHubUser}/node-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                }
            }
        }
        
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                // sh "docker run -d --name node-app -p 8000:8000 node-app"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    post {
      always {
        echo 'Pipeline execution completed'
      }
      success {
        echo 'Successfully deployed the application'
      }
      failure {
        echo 'Deployment failed'
      }
    }
}