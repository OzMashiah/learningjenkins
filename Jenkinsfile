pipeline {
	agent any
	tools {
		go 'go-1.19'
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
		stage('Image build') {
			steps {
				script {
					app = docker.build("adminturneddevops/go-webapp-sample")
				}
			}
		}
		stage('Build') {
			steps {
				git 'https://github.com/AdminTurnedDevOps/go-webapp-sample.git'
				sh 'go build .'
			}
		}
		stage('Run') {
			steps {
				sh 'cd /var/lib/jenkins/workspace/kodekloudtut && go-webapp-sample &'
			}
		}	
	}
}
