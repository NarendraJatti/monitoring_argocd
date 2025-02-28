pipeline {
    agent any
    
    environment {
        GIT_CREDENTIALS_ID = 'your-git-credentials-id'  // Replace with your Jenkins credential ID for Git
        GIT_REPO = 'git@your-repo-url.git'              // Replace with your Git repo URL
        MANIFEST_FILE = 'path/to/your/manifest.yaml'    // Path to the Kubernetes manifest in your repo
        IMAGE_TAG = "${env.BUILD_NUMBER}"              // Using build number as the image tag
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: "${env.GIT_CREDENTIALS_ID}", url: "${env.GIT_REPO}"
            }
        }
        
        stage('Build') {
            steps {
                script {
                    // Replace with your build steps, e.g., Docker image build
                    sh 'docker build -t your-image:${IMAGE_TAG} .'
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    // Replace with your unit test steps
                    sh './run-unit-tests.sh'
                }
            }
        }

        stage('Update Kubernetes Manifest') {
            steps {
                script {
                    // Update the image tag in the Kubernetes manifest
                    sh """
                    sed -i 's|image: your-image:.*|image: your-image:${IMAGE_TAG}|g' ${MANIFEST_FILE}
                    """
                }
            }
        }

        stage('Commit and Push Changes') {
            steps {
                script {
                    // Commit the updated manifest and push to Git
                    sh """
                    git config user.email "jenkins@example.com"
                    git config user.name "Jenkins CI"
                    git add ${MANIFEST_FILE}
                    git commit -m "Update image tag to ${IMAGE_TAG}"
                    git push origin main
                    """
                }
            }
        }
        
        stage('Deploy via ArgoCD') {
            steps {
                script {
                    // Trigger ArgoCD sync (assuming ArgoCD CLI is installed in Jenkins)
                    sh 'argocd app sync your-app-name'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
