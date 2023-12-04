pipeline {
    agent any
    environment {
        //be sure to replace "felipelujan" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "jhndbr/deploys"
        KUBECONFIG = credentials('kubeconfig')
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
          

       stages {
        stage('Desplegar en Kubernetes') {
            steps {
                kubeconfig(
                    credentialsId: KUBECONFIG,
                    serverUrl: 'https://192.168.58.2:8443'
                ) {
                    script {
                        // Puedes ejecutar comandos de Kubernetes aquí
                        sh 'kubectl apply -f myweb.yaml'
                    }
                }
            }
        }
    }
            }
        }
    }
