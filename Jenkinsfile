pipeline {
    agent any
    environment{
        DOCKERHUB_CREDENTIALS= credentials('kevinospina-dockerhub')
    }
    stages {
         stage('Checkout') {
            steps {
                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], browser: [$class: 'GithubWeb', repoUrl: 'https://github.com/KevinOspina/hello_jest.git'], extensions: [], userRemoteConfigs: [[credentialsId: '5588d1ab-8ef0-45f4-961a-85d26e404591', url: 'https://github.com/KevinOspina/hello_jest']]])
            }
        }
        
        stage('Build'){
            steps{
                 sh 'docker build -t kevinospina03/hello_jest:latest .'
            }
        }
        
        stage('Login'){
            steps{
                
                sh 'echo $PASS | docker login -u $USER --password-stdin'
                 //sh '''
                 //    echo "${PASS} | docker login -u ${USER} --password-stdin"
                 //'''
            }
        }
        
        stage('Push'){
            steps{
                 sh 'docker push kevinospina03/hello_jest:latest'
            }
        }
        
        
        stage('Deploy') {
            steps {
               
                withCredentials([usernamePassword(credentialsId: '37267417-47b3-42ac-9844-3f307ddb9306', passwordVariable: 'password', usernameVariable: 'username')]){
                    /**
                    * Restart docker server
                    **/
                    sh '''
                        echo "${PASS} | docker login -u ${USER} --password-stdin"
                        docker stop hello_jest
                        docker rm hello_jest
                        docker pull kevinospina03/hello_jest:latest
                        docker run -d -p 3002:3002 --name hello_jest -t kevinospina03/hello_jest:latest
                    '''
                }
            }
        
        }
    }
    post {
        always {
            sh 'docker logout'
        }

    }

}
