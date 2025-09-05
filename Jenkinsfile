pipeline {
	agent {
		docker {image 'python:3.11-slim'}
	}

	stages {
		stage('checkout') {
			steps {
				checkout scm
			}

		}
stage('build container') {
			steps {
				sh 'docker build -t jenkins-fastapi .'
			}

		}

		stage('Run container') {
			steps {
				sh 'docker run -d -p 8000:8000 --name jenkins-fastapi-container jenkins-fastapi'
			}
		}

		stage('test server') {
			steps {
				sh
				"""
					for i in {1..10}; do
						curl -s http://localhost:8000/ && break
						sleep 2
					done

				"""
			}
		}
	}

	post {
		always {
			// clean up the docker container
			sh 'docker rm -f  jenkins-fastapi-container || true'
		}
	}

}
