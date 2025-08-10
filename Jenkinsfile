pipeline {
    agent any
    
    tools {
        jdk 'jdk17'
        nodejs 'node22'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        IMAGE_TAG = "${BUILD_NUMBER}"
        REGISTRY = "youracr.azurecr.io"
        AZURE_CREDENTIALS = credentials('azure-service-principal') // Jenkins creds
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        
    }
}
