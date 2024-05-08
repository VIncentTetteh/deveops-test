pipeline {
    agent {
       label "ubuntu-agent"
    }

    parameters {
        string(name: 'DB_HOST', defaultValue: 'mysql-wordpress', description: 'Database host')
        string(name: 'DB_NAME', defaultValue: 'wordpress', description: 'Database name')
        string(name: 'DB_USER', defaultValue: 'root', description: 'Database user')
        string(name: 'PASSWORD', defaultValue: 'pass', description: 'Password')
    }
    
    environment {
        NAMESPACE = 'wordpress'
        RELEASE_NAME = "wordpress-release-${BUILD_TAG}"
    }

    stages {
        stage('Checkout') {
            steps {
                
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/VIncentTetteh/deveops-test.git'
            }
        }

        stage('Deploy Helm Chart') {
            steps {
                
                sh "helm upgrade --install ${RELEASE_NAME} . --create-namespace --namespace ${NAMESPACE} --set wordpress.dbHost=${DB_HOST} --set wordpress.dbName=${DB_NAME} --set wordpress.dbUser=${DB_USER} --set wordpress.password=${PASSWORD}"
            }
        }
    }
        
}
