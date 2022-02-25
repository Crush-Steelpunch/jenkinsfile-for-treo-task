pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://git.sr.ht/~baggypants/trio-task']]])
		sh "docker build ."
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
