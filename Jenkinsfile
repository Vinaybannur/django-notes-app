pipeline{

    agent{
        node{
            label "dev"
        }
    }

    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/Vinaybannur/django-notes-app.git", branch: "main"
                echo "Code Clone done"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build -t notes-app-jenkins:latest ."
                echo "Build done"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"dockerId",
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                sh "docker image tag notes-app-jenkins:latest ${env.dockerHubUser}/notes-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app-jenkins:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
                echo "Deployed successfully"
            }
        }
    }
}

