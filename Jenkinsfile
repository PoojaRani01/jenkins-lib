pipeline {
    agent any
    parameters {
        booleanParam(name: 'SKIP_CODE_STABILITY', defaultValue: false, description: 'Skip Code Stability Scan')
        booleanParam(name: 'SKIP_CODE_QUALITY', defaultValue: false, description: 'Skip Code Quality Analysis')
        booleanParam(name: 'SKIP_CODE_COVERAGE', defaultValue: false, description: 'Skip Code Coverage Analysis')
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Parallel Analysis') {
            parallel {
                stage('Code Stability') {
                    when { expression { !params.SKIP_CODE_STABILITY } }
                    steps { echo "Running Code Stability" }
                }
                stage('Code Quality Analysis') {
                    when { expression { !params.SKIP_CODE_QUALITY } }
                    steps { echo "Running Code Quality Analysis" }
                }
                stage('Code Coverage Analysis') {
                    when { expression { !params.SKIP_CODE_COVERAGE } }
                    steps { echo "Running Code Coverage Analysis" }
                }
            }
        }
        stage('Approval') {
            steps {
                script {
                    def userInput = input(message: "Approve publishing?", parameters: [choice(name: 'Approve?', choices: ['Yes','No'])])
                    if(userInput == 'No') { error("Publishing Aborted") }
                }
            }
        }
        stage('Publish Artifacts') { steps { echo "Publishing Artifacts..." } }
    }
    post {
        success { echo "Build SUCCESSFUL!" }
        failure { echo "Build FAILED!" }
    }
}
