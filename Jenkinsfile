pipeline {
    agent any

    tools {
        // Use the Maven tool configured in Global Tool Configuration
        maven 'Maven 3.x'  // Replace 'Maven 3.x' with the actual name from Global Tool Configuration
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
                echo "Building the code using Maven"
                sh 'mvn clean package'
            }
            post {
                success {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Build status email",
                        body: "Build was successful",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Build status email",
                        body: "Build failed",
                        attachLog: true
                    )
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo "Running unit tests with JUnit"
                sh 'mvn test'
                echo "Running integration tests with Selenium"
            }
            post {
                success {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Test status email",
                        body: "Tests were successful",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Test status email",
                        body: "Tests failed",
                        attachLog: true
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo "Running code analysis with SonarQube"
                // Replace this with your SonarQube analysis command
                sh 'sonar-scanner -Dsonar.projectKey=your-project-key -Dsonar.host.url=http://your-sonarqube-server:9000 -Dsonar.login=your-token'
            }
            post {
                success {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Code analysis status email",
                        body: "Code analysis was successful",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Code analysis status email",
                        body: "Code analysis failed",
                        attachLog: true
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo "Running security scan with OWASP Dependency Check"
                sh 'dependency-check --project "your-project-name" --scan ./'
            }
            post {
                success {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Security scan status email",
                        body: "Security scan was successful",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Security scan status email",
                        body: "Security scan failed",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying to staging server using AWS CodeDeploy"
                sh 'aws deploy create-deployment --application-name YourApp --deployment-group StagingGroup'
            }
            post {
                success {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Deployment to Staging status email",
                        body: "Deployment to Staging was successful",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Deployment to Staging status email",
                        body: "Deployment to Staging failed",
                        attachLog: true
                    )
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo "Running integration tests on the staging environment with Selenium"
                sh 'newman run your-postman-collection.json'
            }
            post {
                success {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Integration test on Staging status email",
                        body: "Integration tests on Staging were successful",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Integration test on Staging status email",
                        body: "Integration tests on Staging failed",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying to production server using AWS CodeDeploy"
                sh 'aws deploy create-deployment --application-name YourApp --deployment-group ProductionGroup'
            }
            post {
                success {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Production deployment status email",
                        body: "Deployment to Production was successful",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.EMAIL_RECIPIENTS}",
                        subject: "Production deployment status email",
                        body: "Deployment to Production failed",
                        attachLog: true
                    )
                }
            }
        }
    }

    post {
        always {
            emailext(
                to: "${env.EMAIL_RECIPIENTS}",
                subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
                body: """<p>Build status: ${currentBuild.currentResult}</p>
                        <p>Check the console output at ${env.BUILD_URL} to view the results.</p>""",
                attachLog: true
            )
        }
    }
}
