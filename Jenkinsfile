pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    


    stages {

        

        stage('Update Helm Chart Values & Push') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'github-creds',  // Jenkins credential ID
                        usernameVariable: 'GIT_USERNAME',
                        passwordVariable: 'GIT_PASSWORD'
                    )]) {
                        sh """
                        # Update image tags in Helm values
                        sed -i "s/tag:.*/tag: $IMAGE_TAG/" charts/frontend/values.yaml
                        sed -i "s/tag:.*/tag: $IMAGE_TAG/" charts/backend/values.yaml
                        sed -i "s/tag:.*/tag: $IMAGE_TAG/" charts/ai/values.yaml

                        # Configure Git (use your GitHub email here, not the Jenkins var)
                        git config --global user.name "$GIT_USERNAME"
                        git config --global user.email "ghaith.benammar709@gmail.com"

                        # Commit and push changes
                        git add .
                        git commit -m "Update image tags to $IMAGE_TAG" || echo "No changes to commit"
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Ghaitho20/Full-Project-Front-Back-end-Deployment---DevOps.git HEAD:main
                        """
                    }
                }
            }
        }
        stage('ArgoCD Sync') {
            steps {
                script {
                    // Argo CD credentials
                    withCredentials([string(credentialsId: 'argocd-password', variable: 'ARGOCD_PASSWORD')]) {
                        sh """
                        # Login to Argo CD CLI
                        argocd login localhost:2020 --username admin --password $ARGOCD_PASSWORD --insecure

                        # Sync all three applications
                        argocd app sync frontend
                        argocd app sync backend
                        argocd app sync ai
                        """
                    }
                }
            }
        }








        
    }
}
