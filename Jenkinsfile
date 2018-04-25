pipeline {
    agent any
    environment {
        BARK_INSTALL_URL=""
    }
    tools {
        maven 'mvn3.5.3'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building ${env.BUILD_ID} on ${env.JENKINS_URL}: ${env.GIT_BRANCH}"
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Test') {
            steps {
                echo "Testing ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh 'mvn verify'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploying To QA') {
            when {
                not {
                    branch 'master'
                }
            }
            steps {
                echo "Deploying ${env.BUILD_ID} to QA on ${env.JENKINS_URL}"
            }
        }
        stage('Deploying To PROD') {
            when {
                branch 'master'
            }
            steps {
                echo "Deploying ${env.BUILD_ID} to PROD on ${env.JENKINS_URL}"
            }
        }
    }
}
