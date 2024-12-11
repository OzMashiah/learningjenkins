pipeline {
    agent none

    stages {
        stage('Checkout code from github') {
            agent any
            steps {
                git 'https://github.com/OzMashiah/learningjenkins.git'
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
                }
            }
            environment {
                MYSQL_HOST = 'localhost'
                MYSQL_USER = 'root'
                MYSQL_PASSWORD = 'root'
                MYSQL_DB = 'test_db'
            }
            steps {
                script {
                sh '''
                        docker exec -i mysql-container mysql -u${MYSQL_USER} -p${MYSQL_PASSWORD} -e "CREATE DATABASE IF NOT EXISTS ${MYSQL_DB};"
                        docker exec -i mysql-container mysql -u${MYSQL_USER} -p${MYSQL_PASSWORD} ${MYSQL_DB} -e "
                            CREATE TABLE IF NOT EXISTS users (
                                id INT PRIMARY KEY,
                                name VARCHAR(50)
                            );
                            INSERT INTO users (id, name) VALUES (1, 'John Doe');
                            INSERT INTO users (id, name) VALUES (2, 'Jane Doe');
                        "
                    '''  
		sh """
                        docker exec -i mysql-container mysql -u${MYSQL_USER} -p${MYSQL_PASSWORD} ${MYSQL_DB} -e "SELECT * FROM users;"
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
