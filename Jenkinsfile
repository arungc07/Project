pipeline {
    agent {
        docker {
            image 'maven:3.8.7-eclipse-temurin-17'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running Maven build...'
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Maven tests...'
                sh 'mvn test'
                sh 'ls -la target/surefire-reports || echo "No test reports found!"'
            }
            post {
                always {
                    junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
                    archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
                }
            }
        }

        stage('Run Spring Boot') {
            steps {
                echo 'Starting Spring Boot app...'
                sh 'nohup mvn spring-boot:run > springboot.log 2>&1 & sleep 15'
                sh 'curl -f http://localhost:8080/docs || echo "Spring Boot not responding."'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building frontend and backend Docker images...'
                dir('frontend') {
                    sh 'docker build -t frontend:latest .'
                }
                dir('backend') {
                    sh 'docker build -t backend:latest .'
                }
            }
        }

        stage('Docker Run') {
            steps {
                echo 'Running Docker containers...'
                sh 'docker run -d -p 8085:8080 --name backend backend:latest'
                sh 'docker run -d -p 8084:8080 --name frontend frontend:latest'
            }
        }
    }
}
