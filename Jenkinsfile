pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/basii23/jenkins-pipeline-demo.git'
        EMAIL_RECIPIENTS = 'basilthankachan98@gmail.com'
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo "Cloning the repository from GitHub: ${env.GIT_REPO_URL}"
                git url: "${env.GIT_REPO_URL}", branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                // Replace this with the actual build commands, e.g., for Maven/Gradle
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo "Running Unit and Integration Tests..."
                // Add commands to run tests, e.g., using JUnit/TestNG
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo "Performing code analysis using SonarQube..."
                // Replace this with SonarQube analysis command
                sh 'sonar-scanner'
            }
        }

        stage('Security Scan') {
            steps {
                echo "Running security scan..."
                // Replace with security scan command, e.g., OWASP Dependency-Check or Snyk
                sh 'dependency-check --project your-project --scan ./'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying to Staging Environment"
                // Replace this with your deployment command for staging (e.g., AWS CLI, Docker)
                // Example for AWS:
                sh 'aws deploy create-deployment --application-name YourApp --deployment-group StagingGroup'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests in Staging..."
                // Add your integration test commands, e.g., Selenium, Postman
                sh 'newman run your-postman-collection.json'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying to Production Environment"
                // Replace this with your production deployment command (e.g., AWS CLI, Docker)
                // Example for AWS:
                sh 'aws deploy create-deployment --application-name YourApp --deployment-group ProductionGroup'
            }
        }
    }

    post {
        success {
            emailext(
                to: "${env.EMAIL_RECIPIENTS}",
                subject: "Pipeline Success",
                body: "The Jenkins pipeline completed successfully."
            )
        }
        failure {
            emailext(
                to: "${env.EMAIL_RECIPIENTS}",
                subject: "Pipeline Failure",
                body: "The Jenkins pipeline failed during one of the stages. Check the attached logs for more details.",
                attachLog: true
            )
        }
    }
}

