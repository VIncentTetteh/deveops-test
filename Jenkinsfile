pipeline {
    agent {
       label "ubuntu-agent"
    }

    parameters {
        string(name: 'DB_HOST', defaultValue: 'mysql-wordpress', description: 'Database host')
        string(name: 'DB_NAME', defaultValue: 'wordpress', description: 'Database name')
        string(name: 'DB_USER', defaultValue: 'root', description: 'Database user')
        string(name: 'DB_SECRET_NAME', defaultValue: 'mysql-secret', description: 'Database secret name')
        string(name: 'PASSWORD', defaultValue: 'pass', description: 'Password')
    }
    
    environment {
        // Set the Kubernetes server URL
        KUBE_SERVER = "https://52.213.150.78:8443"
        // Set the Kubernetes Service Account token
        KUBE_TOKEN = credentials('minikube')
        // Set the namespace where you want to deploy the Helm chart
        NAMESPACE = 'wordpress'
        RELEASE_NAME = "wordpress-release-${BUILD_TAG}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your code from version control (e.g., Git)
                git branch: 'main', credentialsId: 'github', url: 'git@github.com:VIncentTetteh/deveops-test.git'
            }
        }

        stage('Deploy Helm Chart') {
            steps {
                // Authenticate with Minikube
                sh "kubectl config set-cluster minikube --server=${KUBE_SERVER} --insecure-skip-tls-verify=true"
                sh "kubectl config set-credentials user --token=${KUBE_TOKEN}"
                sh "kubectl config set-context minikube --cluster=minikube --user=user"
                sh "kubectl config use-context minikube"

                // Deploy Helm chart to Minikube
                sh "helm upgrade --install ${RELEASE_NAME} ./deveops-test --create-namespace --namespace ${NAMESPACE} --set wordpress.dbHost=${DB_HOST} --set wordpress.dbName=${DB_NAME} --set wordpress.dbUser=${DB_USER} --set wordpress.dbSecretName=${DB_SECRET_NAME} --set wordpress.password=${PASSWORD}"
            }
        }
    }
        
}
