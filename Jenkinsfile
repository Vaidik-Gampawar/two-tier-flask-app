@Library("Shared") _
pipeline{
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                script{
                    clone("https://github.com/Vaidik-Gampawar/two-tier-flask-app", "master")
                }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }
        stage("Build"){
            steps{
                script{
                    docker_build()
                }
                
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
    
post {
    success {
        emailext(
            subject: "Build Successfull",
            body: "Good News: Your build was successful",
            to: 'vdkcodes@gmail.com'
            )
    }
    failure {
        emailext(
            subject: "Build Fail",
            body: "Bad News: Your build was failed",
            to: 'vdkcodes@gmail.com'
            )
    }
}
}
