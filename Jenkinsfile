pipeline {
	agent any
	tools {
		go '1.4'
	}

	environment {
		GO111MODULE='on'
	}

	stages {
		stage('Test') {
			steps {
				git 'https://github.com/AdminTurnedDevOps/go-webapp-sample.git'
				sh 'go test ./...'
			}
		}
	}
}
