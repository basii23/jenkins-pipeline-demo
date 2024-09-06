pipeline {
    agent any

    tools {
        // Use the Maven tool that you configured in Global Tool Configuration
        maven 'Maven 3.x'  // Replace 'Maven 3.x' with the actual name you gave during the Maven setup
    }

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
                echo "Building the project with Maven..."
                sh 'mvn clean package'  // Use Maven to clean and package the project
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo "Running Unit and Integration Tests using Maven..."
                sh 'mvn test'  // Running unit and integration tests using Maven
            }
        }

        stage('Code Analysis') {
            steps {
                echo "Performing code analysis using SonarQube..."
                // Make sure you have SonarQube properly configured for this command
                sh 'sonar-scanner -Dsonar.projectKey=your-project-key -Dsonar.host.url=http://your-sonarqube-server:9000 -Dsonar.login=your-token'
            }
        }

        stage('Security Scan') {
            steps {
                echo "Running security scan with OWASP Dependency Check..."
                // Ensure OWASP Dependency Check is installed and properly configured
                sh 'dependency-check --project "your-project-name" --scan ./'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying to Staging Environment using AWS CLI..."
                // AWS CLI deployment command for staging environment
                sh 'aws deploy create-deployment --application-name YourApp --deployment-group StagingGroup'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests in Staging using Newman..."
                // Newman integration test for staging
                sh 'newman run your-postman-collection.json'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying to Production Environment using AWS CLI..."
                // AWS CLI deployment command for production environment
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
