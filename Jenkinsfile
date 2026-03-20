pipeline{
    agent any;
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/Vaidik-Gampawar/two-tier-flask-app", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t my-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Done by Tester."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                    )]) {
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker image tag my-app ${env.dockerHubUser}/my-app"
                        sh "docker push ${env.dockerHubUser}/my-app"
                    }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build"
            }
        }
    }
}
