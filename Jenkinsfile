pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Code Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/dawoodkal123/java-project.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Artifacts') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('tomcat') {
            steps {
                deploy adapters: [tomcat10(alternativeDeploymentContext: '', credentialsId: 'tomcat', path: '', url: 'http://98.80.142.127:8080/')], contextPath: 'netflix', war: 'target/*.war'
            }
        }
    }
}
