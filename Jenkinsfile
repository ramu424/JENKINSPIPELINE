
pipeline {

    agent any

    tools {
        maven 'MAVEN_INSTALLATION'
        jdk 'JDK_INSTALLATION'
    }

    environment {
        REPO_URL = 'https://github.com/ramu424/maven-web-app.git'
        BRANCH = 'main'
        TOMCAT_URL = 'http://192.168.31.178:9999'
        TOMCAT_PATH = '/maven-web-app'
    }

    stages {

        stage('GET CODE') {
            steps {
                git branch: "${env.BRANCH}",
                    credentialsId: 'github-creds',
                    url: "${env.REPO_URL}"
            }
        }

        stage('BUILD PACKAGE') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('DEPLOY TOMCAT') {
            steps {
                deploy adapters: [
                    tomcat9(
                        credentialsId: 'tomcat-creds',
                        url: "${env.TOMCAT_URL}"
                    )
                ],
                contextPath: "${env.TOMCAT_PATH}",
                war: 'target/*.war'
            }
        }
    }
}
