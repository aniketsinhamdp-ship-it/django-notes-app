@Library('Shared')_
pipeline{
    agent { label 'dev-server'}

    environment {
        DB_HOST = "<YOUR_MYSQL_HOST_OR_RDS_ENDPOINT>"
        DB_PORT = "3306"
        DB_NAME = "<YOUR_DB_NAME>"
        DB_USER = "<YOUR_DB_USER>"
        DB_PASSWORD = "<YOUR_DB_PASSWORD>"
    }

    stages{
        stage("Code clone"){
            steps{
                sh "whoami"
                clone("https://github.com/LondheShubham153/django-notes-app.git","main")
            }
        }
        stage("Code Build"){
            steps{
                dockerbuild("notes-app","latest")
            }
        }
        stage("Push to DockerHub"){
            steps{
                dockerpush("dockerHubCreds","notes-app","latest")
            }
        }
        stage("Deploy"){
            steps{
                sh """
                    docker rm -f notes-app || true
                    docker run -d --name notes-app -p 8000:8000 \\
                      -e DB_HOST="${DB_HOST}" \\
                      -e DB_PORT="${DB_PORT}" \\
                      -e DB_NAME="${DB_NAME}" \\
                      -e DB_USER="${DB_USER}" \\
                      -e DB_PASSWORD="${DB_PASSWORD}" \\
                      <YOUR_DOCKERHUB_USERNAME>/notes-app:latest
                """
            }
        }
    }
}
