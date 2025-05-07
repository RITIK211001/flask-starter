pipeline {
    agent any
    stages {
        stage('Check Docker Daemon Access') {
            steps {
                script {
                    // Check if Docker daemon is accessible and list running containers
                    sh 'docker ps'
                }
            }
        }

        stage('Checkout Code') {
            steps {
                script {
                    // Checkout the code from your forked GitHub repository
                    git branch: 'main', url: 'https://github.com/RITIK211001/flask-starter'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image from the Dockerfile in the repo
                    sh 'docker build -t flask-student .'
                }
            }
        }

        stage('Stop Previous Container') {
            steps {
                script {
                    // Stop any running container named 'flask-student' (if exists)
                    sh '''
                        CONTAINER_ID=$(docker ps -q -f "name=flask-student")
                        if [ -n "$CONTAINER_ID" ]; then
                            docker stop $CONTAINER_ID
                            docker rm $CONTAINER_ID
                        fi
                    '''
                }
            }
        }

        stage('Run New Docker Container') {
            steps {
                script {
                    // Run a new container from the newly built image
                    sh 'docker run -d -p 5000:5000 --name flask-student flask-student'
                }
            }
        }
    }

    post {
        success {
            // Print success message when the pipeline completes successfully
            echo 'Docker container successfully built and deployed!'
        }
        failure {
            // Print failure message if the pipeline fails
            echo 'Pipeline failed. Please check the logs for errors.'
        }
    }
}
