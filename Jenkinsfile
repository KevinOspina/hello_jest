pipeline {
    agent any

    stages {
         stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], browser: [$class: 'GithubWeb', repoUrl: 'https://github.com/KevinOspina/hello_jest.git'], extensions: [], userRemoteConfigs: [[credentialsId: '261d6b1d-4e0d-45f1-96b2-eae3ea36e052', url: 'https://github.com/KevinOspina/hello_jest.git']]])
            }
             
        }
        
        stage('Build') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm install http-server'
                sh 'npm install'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'npm run build'
                sh 'npm run test'
                
            }
            
        }
    }
}
