pipeline {
    agent any 

    environment {
        REGISTRY_URL   = "https://hub.docker.com/repositories/antonchervets"
        IMAGE_NAME     = "my-web-app"
        IMAGE_TAG      = "${BUILD_NUMBER}" // Automatically uses the Jenkins build number as the tag
        CHART_PATH     = "./helm/my-web-app" // EXACT path from your GitHub repository
        RELEASE_NAME   = "my-web-app-release"
        NAMESPACE      = "my-web-app"
    }

    stages {
        stage('Checkout') {
            steps {
                cleanWs() 
                checkout scm 
            }
        }

        stage('Build & Push Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG} ."
                
                echo 'Pushing Docker image to registry...'
                sh "docker push ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }

        stage('Test Helm Chart') {
            steps {
                echo 'Linting Helm chart to check for syntax issues...'
                sh "helm lint ${CHART_PATH}"
                
                echo 'Running a dry-run installation...'
                sh "helm install ${RELEASE_NAME} ${CHART_PATH} --dry-run --debug"
            }
        }

        stage('Deploy with Helm') {
            steps {
                echo 'Deploying application to Kubernetes via Helm...'
                sh """
                    helm upgrade --install ${RELEASE_NAME} ${CHART_PATH} \
                    --namespace ${NAMESPACE} \
                    --create-namespace \
                    --set image.tag=${IMAGE_TAG}
                """
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished. Cleaning up...'
        }
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline Failed. Please check logs.'
        }
    }
}
