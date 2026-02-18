pipeline {
    agent any

    tools {
        jdk 'JDK17'
    }

    environment {
        TOMCAT_URL = "http://54.144.185.171:8081/"
        APP_NAME   = "onlinebookstore"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/ashForsys2025/java_project'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn -B -e clean package'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tomcat-creds',
                                usernameVariable: 'TOMCAT_USER',
                                passwordVariable: 'TOMCAT_PASS')]) {

                    sh """
                    curl -v -u $TOMCAT_USER:$TOMCAT_PASS \
                    -T target/*.war \
                    "$TOMCAT_URL/manager/text/deploy?path=/$APP_NAME&update=true"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Build/Deploy failed!"
        }
    }
}
