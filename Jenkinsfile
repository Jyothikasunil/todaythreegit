pipeline {
    agent any

    environment {
        RECIPIENTS = 'jyothikasunil006@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the code...'
                    // Example using Maven
                    bat 'mvn clean package || exit 1'
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    // Example using Maven and JUnit
                    bat 'mvn test || exit 1'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    echo 'Performing code analysis...'
                    // Example using SonarQube
                    withSonarQubeEnv('SonarQube') {
                        bat 'mvn sonar:sonar || exit 1'
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    // Example using OWASP Dependency-Check
                    bat 'dependency-check.bat --project MyProject --scan ./ || exit 1'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging...'
                    // Example using AWS CLI
                    bat 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name StagingGroup --s3-location bucket=mybucket,bundleType=zip,key=myapp.zip || exit 1'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    // Example of running custom integration test script
                    bat 'run-staging-tests.bat || exit 1'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production...'
                    // Example using AWS CLI
                    bat 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name ProductionGroup --s3-location bucket=mybucket,bundleType=zip,key=myapp.zip || exit 1'
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Pipeline finished. Sending notification emails...'
            }
        }
        success {
            emailext(
                to: env.RECIPIENTS,
                subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName} - SUCCESS",
                body: """<p>Build status: ${currentBuild.currentResult}</p>
                         <p>Check the console output at ${env.BUILD_URL} to view the results.</p>""",
                attachLog: true
            )
        }
        failure {
            emailext(
                to: env.RECIPIENTS,
                subject: "Jenkins Pipeline: ${currentBuild.fullDisplayName} - FAILURE",
                body: """<p>Build status: ${currentBuild.currentResult}</p>
                         <p>Check the console output at ${env.BUILD_URL} to view the results.</p>""",
                attachLog: true
            )
        }
    }
}
