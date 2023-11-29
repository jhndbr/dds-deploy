dockerPipeline(
stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'dockerhub_login') {
       def app = docker.build("jhndbr/dds-deploy", '.').push()
     }
)
