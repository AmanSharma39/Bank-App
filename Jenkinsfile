pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_VERSION', defaultValue: 'latest', description: 'Docker Image Version')
    }

    stages {
        stage("Code Clone") {
            steps {
                git url: "https://github.com/AmanSharma39/Bank-App.git", branch: "start"
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t bankapp-eks:${params.IMAGE_VERSION} ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHub",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {
                    sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                    sh "docker tag bankapp-eks:${params.IMAGE_VERSION} $dockerHubUser/bankapp-eks:${params.IMAGE_VERSION}"
                    sh "docker push $dockerHubUser/bankapp-eks:${params.IMAGE_VERSION}"
                }
            }
        }

        stage("Cleanup") {
            steps {
                sh "docker rmi $dockerHubUser/bankapp-eks:${params.IMAGE_VERSION} || true"
                sh "docker rmi bankapp-eks:${params.IMAGE_VERSION} || true"
            }
        }
    }
}
