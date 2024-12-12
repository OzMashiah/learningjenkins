pipeline {
    agent none

    stages {
        stage('Checkout code from GitHub') {
            agent any
            steps {
                git branch: 'main', url: 'https://github.com/OzMashiah/learningjenkins.git'
            }
        }

        stage('Java Script') {
            agent any  // Use 'any' or specify an agent where the steps will run
            steps {
                script {
                    node {
                        // Run the node container in detached mode and keep it alive with tailing logs
                        def container = docker.image('node:14').run('-d tail -f /dev/null')
                        try {
                            // Copy the script to the container
                            sh "docker cp script.js ${container.id}:script.js"
                            // Execute the script inside the container
                            sh "docker exec ${container.id} node script.js"
                        } finally {
                            // Clean up and stop the container
                            sh "docker stop ${container.id}"
                            sh "docker rm ${container.id}"
                        }
                    }
                }
            }
        }

        stage('Run MySQL Query') {
            environment {
                MYSQL_USER = 'root'
                MYSQL_DB = 'test_db'
                MYSQL_ALLOW_EMPTY_PASSWORD = 'yes'
            }
            agent any  // Use 'any' or specify an agent
            steps {
                script {
                    node {
                        docker.image('mysql:8').withRun('-p 3306:3306 --network host') { c ->
                            sh "docker exec ${c.id} bash -c 'mysql -e \"SELECT 1\"'"
                            sh "docker exec ${c.id} bash -c 'cat /etc/os-release'"
                        }
                    }
                }
            }
        }
    }
}

