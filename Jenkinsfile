pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        dockerTool "docker"
        
    }

    stages {
        stage('VCS') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/creepyghost/hello-world-webapp'
            }
        }
        stage('Setup') {
            steps {
                script {
                    sh """
                    pip install -r requirements.txt
                    gunicorn app:app
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: '',
                        toolName: 'docker') {
                        
                        // Build and Push
                        def echoServerImage = docker.build("bharat/hello-world-webapp:latest");
                        echoServerImage.push();
                    }
                }
            }
        }
    }
    post {
        success {
            echo "Success"
        }
        failure {
            echo "Failure!"
        }
    }
}
