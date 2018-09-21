pipeline {
    agent any

	parameters {
        string(description: 'Parameter', name: 'DOCKER_USERNAME', defaultValue: 'dockeruser')
    }	
    stages {
		stage('RunCleanups') {
            steps {
		sh "docker stop mynginx"
                sh "docker rmi \$(docker images -q) -f"
                sh "docker rm \$(docker ps -a -q)"
            }
		}
        stage('DockerImageBuild') {
            steps {
                sh "cd /data/mydocker1/mydocker"
				sh "chmod 755 index.html"
                sh "docker build -t mynginximage ."
            }
		}
		stage('RunContainer') {
            steps {
                sh "docker run -d --name mynginx -p 8081:80 mynginximage"
				sh "sleep 30"
                sh "curl 192.168.56.101:8081"
            }
		}
		stage('UploadContainer') {
            steps {
                sh "docker stop mynginx"
                sh "docker commit mynginx mynginxtag:1.0"
                sh "docker tag mynginxtag:1.0 $DOCKER_USERNAME/mynginxdockerremote"
                sh "docker image push $DOCKER_USERNAME/mynginxdockerremote"
            }	
        }
    }
}
