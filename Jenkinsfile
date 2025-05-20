pipeline {
    agent any

    parameters {
        booleanParam(name: 'SKIP_TEST', defaultValue: false, description: 'Skip Unit Tests')
        booleanParam(name: 'SKIP_COVERAGE', defaultValue: false, description: 'Skip Coverage')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                git 'https://github.com/Rajshekhar98/jenkins-repo.git'
            }
        }

        stage('Build & Analyze') {
            parallel {
                stage('Unit Tests') {
                    when { not { expression { params.SKIP_TEST } } }
                    steps {
                        echo 'Running Unit Tests...'
                        sh './mvnw test'
                    }
                }
                stage('Code Coverage') {
                    when { not { expression { params.SKIP_COVERAGE } } }
                    steps {
                        echo 'Running Code Coverage...'
                        sh './mvnw verify'
                    }
                }
            }
        }

        stage('Approval') {
            steps {
                input message: 'Approve to publish artifacts?', ok: 'Publish'
            }
        }

        stage('Publish') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                echo 'Artifacts published!'
            }
        }
    }
}

