pipeline {
    agent none

    stages {
        stage('Checkout code from GitHub') {
            agent any
            steps {
                git branch: 'main', url: 'https://github.com/OzMashiah/learningjenkins.git'
            }
        }
	

        stage('Run MySQL Query') {
            environment {
                MYSQL_USER = 'root'
                MYSQL_DB = 'test_db'
                MYSQL_ALLOW_EMPTY_PASSWORD = 'yes'
            }
            steps {
                script {
                    docker.image('mysql:8').withRun('-p 3306:3306 --network host') { c ->
			sh "docker exec ${c.id} bash -c 'mysql -e 'SELECT 1''"		
			sh "docker exec ${c.id} bash -c 'cat /etc/os-release'"
                    				}
                			}
            			}
       	 		}
    		}
	}
