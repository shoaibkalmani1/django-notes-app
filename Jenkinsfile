pipeline{
    agent any
    stages{
        stage("cloning"){
            steps{
                echo "cloning the clone"
                git url:"https://github.com/shoaibkalmani1/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the code"
                sh "docker build -t image ."
            }
        }
        stage("pushing to dockerHub"){
            steps{
                echo "pushing the code to dockerHub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}" 
                sh "docker tag image ${env.dockerHubUser}/newimage:latest"
                sh "docker push ${env.dockerHubUser}/newimage:latest"
                }
            }
        }
        stage("Deployment"){
            steps{
                echo "Deploying the code"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
