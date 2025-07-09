pipeline {
    agent any

    stages {
        stage('Build & Test') {
            steps {
                script {
                    docker.image('maven:3.8.7-eclipse-temurin-17').inside('-v /var/run/docker.sock:/var/run/docker.sock') {
                        sh 'mvn clean install -DskipTests'
                        sh 'mvn test'
                        sh 'ls -la target/surefire-reports || echo "No test reports found!"'
                    }
                }
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
                sh 'nohup mvn spring-boot:run > springboot.log 2>&1 & sleep 15'
                sh 'curl -f http://localhost:8080/docs || echo "Spring Boot not responding."'
            }
        }

        stage('Docker Build') {
            steps {
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
                sh 'docker run -d -p 8085:8080 --name backend backend:latest'
                sh 'docker run -d -p 8084:8080 --name frontend frontend:latest'
            }
        }
    }
}
