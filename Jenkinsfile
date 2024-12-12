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
                    // Run MySQL container in detached mode with port 3306 mapped to host
                    docker.image('mysql:8-oracle').withRun('-p 3306:3306 --network host') { c ->
			sh 'docker exec ${c.id} whoami'		
			sh 'docker exec ${c.id} cat /etc/os-release'
                    }
                }
            }
        }
    }
}
