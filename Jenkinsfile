pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    environment {
        TOMCAT_IP   = "localhost"
        TOMCAT_PORT = "8081"
        APP_NAME    = "onlinebookstore"
        WAR_FILE    = "target/onlinebookstore.war"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/ashForsys2025/java_project'
            }
        }

        stage('Build using Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
                sh """
                curl -v -u admin:admin123 \
                -T ${WAR_FILE} \
                "http://${TOMCAT_IP}:${TOMCAT_PORT}/manager/text/deploy?path=/${APP_NAME}&update=true"
                """
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
