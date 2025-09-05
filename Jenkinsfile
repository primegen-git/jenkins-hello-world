pipeline {
	agent any // so that jenkins can use the docker-ce-cli that is being installed with custom jenkins dockerfile.

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
				sh """
					for i in {1..10}; do
						RESPONSE=\$(http://localhost:8000/)
						if [ ! -z "\$RESPONSE" ]; then
							echo "fastapi reponse: \$RESPONSE"
							break
						fi
						echo "waiting for server to start!!!!"
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
