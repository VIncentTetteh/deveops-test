pipeline {
    agent ubuntu-agent
    
    environment {
        // Set the Kubernetes server URL
        KUBE_SERVER = "https://192.168.103.2:8443"
        // Set the Kubernetes Service Account token
        KUBE_TOKEN = credentials('kubernetes-token')
        // Set the namespace where you want to deploy the Helm chart
        NAMESPACE = 'your-namespace'
        RELEASE_NAME = "your-release-" + new Date().format("yyyyMMddHHmmss")
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your code from version control (e.g., Git)
                git credentialsId: 'github-credentials', url: 'https://github.com/VIncentTetteh/deveops-test.git'
            }
        }
    }

    stages {
        stage('Deploy Helm Chart') {
            steps {
                // Authenticate with Minikube
                sh "kubectl config set-cluster minikube --server=${KUBE_SERVER} --insecure-skip-tls-verify=true"
                sh "kubectl config set-credentials user --token=${KUBE_TOKEN}"
                sh "kubectl config set-context minikube --cluster=minikube --user=user"
                sh "kubectl config use-context minikube"

                // Deploy Helm chart to Minikube
                sh "helm upgrade --install ${RELEASE_NAME} ./deveops-test/ --namespace ${NAMESPACE} --set wordpress.dbHost=mysql-wordpress --set wordpress.dbName=wordpress --set wordpress.dbUser=root --set wordpress.dbSecretName=mysql-secret --set wordpress.password=pass"}
        }
    }
        
}
