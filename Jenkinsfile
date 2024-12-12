pipeline {
    agent none

    stages {
        stage('Checkout code from github') {
            agent any
            steps {
                git branch: 'main', url: 'https://github.com/OzMashiah/learningjenkins.git'
            }
        }

        stage('Run JavaScript') {
            agent {
                docker {
                    image 'node:14'
                }
            }
            steps {
                script {
                    sh 'ls -l'
                    sh 'node script.js'
                }
            }
        }

        stage('Run MySQL Query') {
            agent {
                docker {
                    image 'mysql:8'
		    args '-u root:root -e MYSQL_ROOT_PASSWORD=root'
                }
            }
            environment {
                MYSQL_USER = 'root'
                MYSQL_DB = 'test_db'
		MYSQL_ALLOW_EMPTY_PASSWORD = true
            }
            steps {
                script {
		// Ensure MySQL is initialized before running commands
		sh 'ls -l /var/lib/mysql/'
		sh 'ls -l /var/run/mysqld'	
		sh '/usr/sbin/mysqld status'
		sh '/usr/sbin/mysqld start'
		sh 'sleep 15'
		
                sh '''
                        mysql -u ${MYSQL_USER} -e "CREATE DATABASE IF NOT EXISTS ${MYSQL_DB};"
                        mysql -u ${MYSQL_USER} ${MYSQL_DB} -e "
                            CREATE TABLE IF NOT EXISTS users (
                                id INT PRIMARY KEY,
                                name VARCHAR(50)
                            );
                            INSERT INTO users (id, name) VALUES (1, 'John Doe');
                            INSERT INTO users (id, name) VALUES (2, 'Jane Doe');
                        "
                    '''  
		sh """
                        mysql -u ${MYSQL_USER} ${MYSQL_DB} -e "SELECT * FROM users;"
                    """
                }
            }
        }
    }

    post {
        always {
            // Clean up resources after the build
            echo 'Cleaning up resources...'
        }
    }
}
