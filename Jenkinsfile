pipeline {
    agent any

    stages {
        stage('DockerImageBuild') {
            steps {
                sh "cd /data/mydocker1/mydocker"
                sh "docker build -t mynginximage ."
                sh "docker run -d --name mynginx -p 8081:80 mynginximage"
                sh "curl 192.168.56.101:8081"
                sh "docker stop mynginx"
                sh "docker commit mynginx mynginxtag:1.0"
                sh "docker tag mynginxremote:1.0 $DOCKER_USERNAME/mynginxdockerremote
                sh "docker image push $DOCKER_USERNAME/mynginxdockerremote"
            }
        }
    }
}
