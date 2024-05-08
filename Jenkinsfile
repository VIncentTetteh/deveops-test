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
        NAMESPACE = 'wordpress'
        RELEASE_NAME = "wordpress-release-${BUILD_TAG}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your code from version control (e.g., Git)
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/VIncentTetteh/deveops-test.git'
            }
        }

        stage('Deploy Helm Chart') {
            steps {
                // Deploy Helm chart to Minikube
                sh "helm upgrade --install ${RELEASE_NAME} . --create-namespace --namespace ${NAMESPACE} --set wordpress.dbHost=${DB_HOST} --set wordpress.dbName=${DB_NAME} --set wordpress.dbUser=${DB_USER} --set wordpress.dbSecretName=${DB_SECRET_NAME} --set wordpress.password=${PASSWORD}"
            }
        }
    }
        
}
