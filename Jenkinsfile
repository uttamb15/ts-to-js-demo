pipeline {
    agent any

    // NodeJS configured in Jenkins
    tools {
        nodejs 'NodeJS'
    }

    environment {
        // Base directory where JS output will be stored
        BASE_OUTPUT_DIR = 'C:\\ts-js-output'

        // Branch name is automatically provided by Multibranch Pipeline
        CURRENT_BRANCH = "${env.BRANCH_NAME}"

        // Final output path per environment
        LOCAL_OUTPUT_DIR = "C:\\ts-js-output\\${env.BRANCH_NAME}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                // Multibranch pipeline already checks out the correct branch
                echo "Checked out branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Compile TypeScript') {
            steps {
                bat 'npx tsc'
            }
        }

        stage('Store JS Locally (Branch-wise)') {
            steps {
                
                bat """
                    echo Storing compiled JS for branch: %CURRENT_BRANCH%

                    REM Create base directory if not exists
                    if not exist "%BASE_OUTPUT_DIR%" (
                        mkdir "%BASE_OUTPUT_DIR%"
                    )

                    REM Create branch-specific directory
                    if not exist "%LOCAL_OUTPUT_DIR%" (
                        mkdir "%LOCAL_OUTPUT_DIR%"
                    )

                    REM Copy compiled JS files
                    copy /Y dist\\*.js "%LOCAL_OUTPUT_DIR%\\"
                """
            }
        }

    }

    post {
        success {
            echo "Build successful for branch: ${env.BRANCH_NAME}"
        }
        failure {
            echo "Build failed for branch: ${env.BRANCH_NAME}"
        }
    }
}
