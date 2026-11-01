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
        stage('Tomcat Deploy') {
            steps {
                // Option A: If Jenkins and Tomcat are on the same machine/server:
                sh 'cp target/NETFLIX-1.2.2.war /var/lib/tomcat10/webapps/netflix.war'
                
                /* Option B: If deploying to a remote server via SSH/Password, 
                   you can use sshpass or scp inside the sh step:
                   sh 'sshpass -p "your_password" scp target/NETFLIX-1.2.2.war ubuntu@98.80.142.127:/var/lib/tomcat10/webapps/netflix.war'
                */
            }
        }
    }
}
