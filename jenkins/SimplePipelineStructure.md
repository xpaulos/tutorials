pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
	         // bash/shell commands
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
	         // bash/shell commands
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
	         // bash/shell commands
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}