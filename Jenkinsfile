pipeline {
  agent any
  environment {
    // Asegúrese de reemplazar "felipelujan" con su propio nombre de usuario de Docker Hub
    DOCKER_IMAGE_NAME = "jhndbr/deploys"
    KUBECONFIG = credentials('kubeconfig')
  }
  stages {
    stage('Build Docker Image') {
      when {
        branch 'main'
      }
      steps {
        node {
          checkout scm
          script {
            docker.withServer('tcp://swarm.example.com:2376', 'swarm-certs') {
              app = docker.build(DOCKER_IMAGE_NAME)
            }
          }
        }
      }
    }
    stage('Push Docker Image') {
      when {
        branch 'main'
      }
      steps {
        node {
          checkout scm
          script {
            docker.withServer('tcp://swarm.example.com:2376', 'swarm-certs') {
              docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_login') {
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
              }
            }
          }
        }
      }
    }
    stage('DeployToProduction') {
      when {
        branch 'main'
      }
    }
    stage('Deploy to Kubernetes') {
      when {
        branch 'main'
      }
      steps {
        node {
          checkout scm
          script {
            // Configura el archivo kubeconfig
            withEnv(["KUBECONFIG=${KUBECONFIG}"]) {
              // Aplica la configuración de Kubernetes
              sh 'kubectl apply -f myweb.yaml'
            }
          }
        }
      }
    }
  }
}
