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
                        // Wait for MySQL to initialize by trying to connect with the mysql command
                        waitUntil {
                            try {
                                // Try to run a simple query to check if MySQL is available
                                sh(script: 'mysql -u ${MYSQL_USER} -h127.0.0.1 -P3306 -e "SELECT 1;"', returnStatus: true) == 0
                            } catch (e) {
                                return false
                            }
                        }

                        // Create Database and Table, then insert values
                        sh '''
                            mysql -u ${MYSQL_USER} -h127.0.0.1 -P3306 -e "CREATE DATABASE IF NOT EXISTS ${MYSQL_DB};"
                            mysql -u ${MYSQL_USER} -h127.0.0.1 -P3306 ${MYSQL_DB} -e "
                                CREATE TABLE IF NOT EXISTS users (
                                    id INT PRIMARY KEY,
                                    name VARCHAR(50)
                                );
                                INSERT INTO users (id, name) VALUES (1, 'John Doe');
                                INSERT INTO users (id, name) VALUES (2, 'Jane Doe');
                            "
                        '''

                        // Query the Database to verify insertion
                        sh '''
                            mysql -u ${MYSQL_USER} -h127.0.0.1 -P3306 ${MYSQL_DB} -e "SELECT * FROM users;"
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up resources after the build
            echo 'Cleaning up resources...'
            // In case the MySQL container is still running, stop it.
            sh 'docker ps -q --filter "ancestor=mysql:8" | xargs -r docker stop'
        }
    }
}

