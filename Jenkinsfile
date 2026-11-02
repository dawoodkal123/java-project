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
        stage('Remote Tomcat Deploy') {
            steps {
                sh '''
                    # Safely locate the generated WAR file
                    WAR_FILE=$(ls target/*.war | head -n 1)
                    echo "Found WAR file: $WAR_FILE"
                    
                    # Copy the WAR file to the remote Tomcat server via SCP
                    # (Make sure your SSH key is configured between Jenkins and the remote server)
                    scp -o StrictHostKeyChecking=no "$WAR_FILE" ubuntu@98.80.142.127:/tmp/netflix.war
                    
                    # Remotely move the WAR file into Tomcat webapps and restart Tomcat
                    ssh -o StrictHostKeyChecking=no ubuntu@98.80.142.127 '
                        sudo mkdir -p /var/lib/tomcat10/webapps
                        sudo mv /tmp/netflix.war /var/lib/tomcat10/webapps/netflix.war
                        sudo chown tomcat:tomcat /var/lib/tomcat10/webapps/netflix.war || sudo chown tomcat10:tomcat10 /var/lib/tomcat10/webapps/netflix.war || true
                        sudo chmod 644 /var/lib/tomcat10/webapps/netflix.war
                        sudo systemctl restart tomcat10
                    '
                '''
            }
        }
    }
}
