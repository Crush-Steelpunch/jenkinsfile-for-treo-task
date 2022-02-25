pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://git.sr.ht/~baggypants/trio-task']]])
		sh "docker build -t leonrobinson/flask-app:latest flask-app/."
		sh "docker tag leonrobinson/flask-app:latest leonrobinson/flask-app:$BUILD_DISPLAY_NAME"
		sh "docker build -t leonrobinson/db:latest db/."
		sh "docker tag leonrobinson/db:latest leonrobinson/db:$BUILD_DISPLAY_NAME"
            }
        }
	stage('Network') {
	    steps {
	    	sh "if ! docker network inspect treonet > /dev/null 2>&1; then docker create network treonet; fi" 
	    }
	}
        stage('Deploy') {
            steps {
                sh "if docker inspect  flask-app > /dev/null 2>&1 ; then docker stop flask-app; docker rm flask-app; fi"
		sh "docker run -d --name flask-app flaskapp"
            }
        }
    }
}
