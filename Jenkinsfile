
pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Code Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/dawoodkal123/java-project.git'
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
                    set -e

                    # SSH Configuration
                    SSH_KEY="$HOME/.ssh/tomcat_deploy"
                    TOMCAT_USER="ubuntu"
                    TOMCAT_IP="98.80.142.127"

                    # Locate WAR file
                    WAR_FILE=$(find target -maxdepth 1 -name "*.war" | head -n 1)

                    if [ -z "$WAR_FILE" ]; then
                        echo "ERROR: WAR file not found!"
                        exit 1
                    fi

                    echo "Deploying: $WAR_FILE"

                    # Secure WAR transfer
                    scp -i "$SSH_KEY" \
                        -o StrictHostKeyChecking=no \
                        "$WAR_FILE" \
                        ${TOMCAT_USER}@${TOMCAT_IP}:/tmp/netflix.war

                    # Remote Tomcat deployment
                    ssh -i "$SSH_KEY" \
                        -o StrictHostKeyChecking=no \
                        ${TOMCAT_USER}@${TOMCAT_IP} '

                        sudo mv /tmp/netflix.war \
                            /var/lib/tomcat10/webapps/netflix.war

                        sudo chmod 644 \
                            /var/lib/tomcat10/webapps/netflix.war

                        sudo systemctl restart tomcat10

                    '

                    echo "Tomcat deployment completed!"
                '''
            }
        }
    }
}
