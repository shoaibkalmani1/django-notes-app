pipeline {
    agent any 
    stages{
        stage("cloneCode"){
            steps {
                 echo "Cloning the code"
                 git url:"https://github.com/shoaibkalmani1/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps {
                echo "building the code"
                sh "docker build -t mynode-app ."
            }
        }
        stage("Push  to Docker Hub"){
            steps {
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerhubPass", usernameVariable:"dockerhubUser")]){
                sh "docker tag mynode-app ${env.dockerhubUser}/mynode-app:latest"    
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker push ${env.dockerhubUser}/mynode-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the conatiner"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
