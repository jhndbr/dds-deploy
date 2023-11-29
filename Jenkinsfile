pipeline {
    agent any
    
    environment {
        // Define las variables necesarias
        DOCKER_IMAGE_NAME = 'imagenuno'
        DOCKER_HUB_CREDENTIALS = 'dockerhub_login'
    }

    stages {
        stage('Clonar Repositorio') {
           steps {
                // Clona tu repositorio desde GitHub utilizando la credencial de GitHub
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: true, recursiveSubmodules: true, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: GITHUB_CREDENTIALS, url: 'https://github.com/jhndbr/dds-deploy.git']]])
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                script {
                    // Construir la imagen Docker
                    docker.build(DOCKER_IMAGE_NAME)
                }
            }
        }

        stage('Subir Imagen a Docker Hub') {
            steps {
                script {
                    // Iniciar sesión en Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        // Subir la imagen al repositorio de Docker Hub
                        docker.image(DOCKER_IMAGE_NAME).push()
                    }
                }
            }
        }
    }
