/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'maven:3.9.12-eclipse-temurin-21-alpine' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
Jenkinsfile (Declarative Pipeline)
/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'node:24.13.0-alpine3.23' } }
    stages {
        stage('build') {
            steps {
                sh 'node --version'
            }
        }
    }
}
Jenkinsfile (Declarative Pipeline)
/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'python:3.14.2-alpine3.23' } }
    stages {
        stage('build') {
            steps {
                sh 'python --version'
            }
        }
    }
}

Jenkinsfile (Declarative Pipeline)
/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'golang:1.25.6-alpine3.23' } }
    stages {
        stage('build') {
            steps {
                sh 'go version'
            }
        }
    }
}