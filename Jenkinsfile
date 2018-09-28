pipeline {
    agent any
	
	    parameters {
        string(defaultValue: "$emailRecipients",
                description: 'List of email recipients',
                name: 'EMAIL_RECIPIENTS')
}
	
	
    stages {
        stage('DockerImageBuild') {
            steps {
		 sh "docker stop $(docker ps -q) 2&>1 /dev/null"
		 sh "docker rm $(docker ps -a -q)  -f 2&>1 /dev/nulldocker"
		 sh "docker rmi $(docker images -q) -f 2&>1 /dev/null"
                sh "cd /root/.jenkins/workspace/Myproject_pipeline"
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
				    post {
						always {
						emailext body: 'Jenkins Pipeline Status',
						attachLog: true,
						compressLog: true,
						mimeType: 'text/html',
						subject: "Pipeline Build ${BUILD_NUMBER}",
						to: "${params.EMAIL_RECIPIENTS}"
}
            }	
        }
    }
}
