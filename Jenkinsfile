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
                echo "Building the project with Maven..."
                sh 'mvn clean package'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo "Running Unit and Integration Tests using Maven..."
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo "Performing code analysis using SonarQube..."
                sh 'sonar-scanner -Dsonar.projectKey=your-project-key -Dsonar.host.url=http://your-sonarqube-server:9000 -Dsonar.login=your-token'
            }
        }

        stage('Security Scan') {
            steps {
                echo "Running security scan with OWASP Dependency Check..."
                sh 'dependency-check --project "your-project-name" --scan ./'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying to Staging Environment using AWS CLI..."
                sh 'aws deploy create-deployment --application-name YourApp --deployment-group StagingGroup'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests in Staging using Newman..."
                sh 'newman run your-postman-collection.json'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying to Production Environment using AWS CLI..."
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
