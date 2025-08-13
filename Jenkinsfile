pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    


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

        stage('SonarQube Analysis') {
            parallel {
                stage('Frontend') {
                    steps {
                        dir('pcd_front') {
                            withSonarQubeEnv('sonar-server') {
                                sh """ 
                                $SCANNER_HOME/bin/sonar-scanner \
                                -Dsonar.projectName=frontend \
                                -Dsonar.projectKey=frontend
                                """
                            }
                        }
                    }
                }
                stage('Backend') {
                    steps {
                        dir('pcd_back/backend') {
                            withSonarQubeEnv('sonar-server') {
                                sh """
                                ./mvnw clean compile sonar:sonar \
                                -Dsonar.projectName=backend \
                                -Dsonar.projectKey=backend \
                                -Dsonar.java.binaries=target/classes
                                """
                            }
                        }
                    }
                }
                stage('AI Service') {
                    steps {
                        dir('ai') {
                            withSonarQubeEnv('sonar-server') {
                                sh """
                                $SCANNER_HOME/bin/sonar-scanner \
                                -Dsonar.projectName=ai \
                                -Dsonar.projectKey=ai
                                """
                            }
                        }
                    }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: true, credentialsId: 'Sonar-token'
                }
            }
        }

        
    }
}
