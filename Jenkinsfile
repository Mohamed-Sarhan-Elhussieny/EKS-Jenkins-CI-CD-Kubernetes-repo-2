pipeline {
    agent {
        label 'kaniko-builder'
    }
    environment {
        DOCKER_IMAGE = 'mohameds2/page'
        IMAGE_TAG = 'latest'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build & Push Image') {
            steps {
                container('kaniko') {
                    sh """
                    /kaniko/executor \
                      --context=\${WORKSPACE} \
                      --dockerfile=\${WORKSPACE}/Dockerfile \
                      --destination=\${DOCKER_IMAGE}:\${IMAGE_TAG} \
                      --cache=true
                    """
                }
            }
        }
        stage('Deploy to App') {
            steps {
                container('kubectl') {
                    sh """
                    kubectl set image deployment/app-deploy \
                      app=\${DOCKER_IMAGE}:\${IMAGE_TAG} \
                      -n app
                    
                    kubectl rollout restart deployment/app-deploy -n app
                    
                    kubectl rollout status deployment/app-deploy -n app
                    """
                }
            }
        }
    }
    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}