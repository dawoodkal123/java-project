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
                sh '''
                    sudo cp target/*.war /var/lib/tomcat10/webapps/netflix.war
                    sudo chown tomcat:tomcat /var/lib/tomcat10/webapps/netflix.war
                    sudo chmod 644 /var/lib/tomcat10/webapps/netflix.war
                    sudo systemctl restart tomcat10
                '''
            }
        }
    }
}
