pipeline {
	agent any
	tools {
		go 'go-1.19'
	}

	environment {
		GO111MODULE='on'
	}

	stages {
        	stage('Install GCC') {
            		steps {
                		script {
                    			// Install gcc if not installed
                    			sh '''
                    			if ! command -v gcc &> /dev/null
                    			then
                        			echo "GCC not found, installing..."
                        			apt update
                        			apt install -y build-essential
                    			fi
                    			'''
                		}
            		}
		}
		stage('Test') {
			steps {
				git 'https://github.com/AdminTurnedDevOps/go-webapp-sample.git'
				sh 'go test ./...'
			}
		}
	}
}
