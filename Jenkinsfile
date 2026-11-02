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
                    # Safely locate the generated WAR file
                    WAR_FILE=$(ls target/*.war | head -n 1)
                    echo "Found WAR file: $WAR_FILE"
                    
                    # Ensure the Tomcat webapps directory exists
                    sudo mkdir -p /var/lib/tomcat10/webapps
                    
                    # Copy WAR file and make it readable for Tomcat
                    sudo cp "$WAR_FILE" /var/lib/tomcat10/webapps/netflix.war
                    sudo chmod 644 /var/lib/tomcat10/webapps/netflix.war
                    
                    # Restart Tomcat to auto-deploy the app
                    sudo systemctl restart tomcat10
                '''
            }
        }
    }
}
