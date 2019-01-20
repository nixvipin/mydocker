pipeline {
    agent any
	
    stages {
        stage('DockerImageBuild') {
            steps {
            	sh "cd /root/.jenkins/workspace/$JOB_NAME"
		sh "chmod 755 index.html"
            	sh "docker build -t mynginximage ."
            }
		}
		stage('RunContainer') {
            steps {
		sh '''
		IPADDR1=\$(ip a | grep -A3 eth0 | head -3 | tail -1 | awk '{print \$2}' | awk -F "/" '{print \$1}')
                docker run -d --name mynginx -p 8081:80 mynginximage
		sleep 15
		echo $IPADDR1
		curl $IPADDR1:8081
		'''
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
