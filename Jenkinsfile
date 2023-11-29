pipeline {
    agent any
    environment {
        //be sure to replace "felipelujan" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "jhndbr/deploys"
    }
    stages {      
        stage('Build Docker Image') {
            when {
                branch 'main'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'main'
            }
           steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubernet_login")
        }
      }
            }
        }
    }
