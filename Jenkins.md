pipeline {
    agent any

    environment {
        MAVEN_HOME = tool name: 'M3', type: 'ToolLocation'
        JAVA_HOME = tool name: 'JDK11', type: 'ToolLocation'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run Maven to build the application
                    sh "'${MAVEN_HOME}/bin/mvn' clean install"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run Maven tests
                    sh "'${MAVEN_HOME}/bin/mvn' test"
                }
            }
        }

        stage('Static Analysis') {
            steps {
                script {
                    // Run static code analysis (e.g., Checkstyle, PMD, or SonarQube)
                    sh "'${MAVEN_HOME}/bin/mvn' checkstyle:check"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy to your environment (e.g., a test server, Docker container, etc.)
                    // This is a placeholder for your actual deployment process
                    sh 'echo "Deploying to server..."'
                }
            }
        }
    }

    post {
        always {
            // Archive build logs and other artifacts
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
        success {
            // Actions on successful build (e.g., notifications)
            echo 'Build and tests passed!'
        }
        failure {
            // Actions on failed build (e.g., notifications)
            echo 'Build or tests failed.'
        }
    }
}
