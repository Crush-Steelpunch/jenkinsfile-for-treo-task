pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://git.sr.ht/~baggypants/trio-task']]])
		sh "docker build -t leonrobinson/flask-app:latest flask-app/."
		sh "docker tag leonrobinson/flask-app:latest leonrobinson/flask-app:$BUILD_NUMBER"
		sh "docker build -t leonrobinson/db:latest db/."
		sh "docker tag leonrobinson/db:latest leonrobinson/db:$BUILD_NUMBER"
            }
        }
	stage('Network') {
	    steps {
	    	sh "if ! docker network inspect treonet > /dev/null 2>&1; then docker network create treonet; fi" 
	    }
	}
        stage('Deploy') {
            steps {
                sh "if docker inspect  flask-app > /dev/null 2>&1 ; then docker stop flask-app; docker rm flask-app; fi"
		sh "docker run -d --network treonet --name flask-app leonrobinson/flask-app"
                sh "if docker inspect  mysql > /dev/null 2>&1 ; then docker stop mysql; docker rm mysql; fi"
		sh "docker run -d --network treonet --name mysql leonrobinson/db"
            }
        }
    }
}
