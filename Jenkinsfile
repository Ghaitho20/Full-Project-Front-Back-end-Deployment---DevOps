pipeline {
    agent any

    


    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Ghaitho20/Full-Project-Front-Back-end-Deployment---DevOps.git'
            }
        }

        
    }
}
