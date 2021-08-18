pipeline {
    agent any

    stages {
         stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], browser: [$class: 'GithubWeb', repoUrl: 'https://github.com/KevinOspina/node-hello.git'], extensions: [], userRemoteConfigs: [[credentialsId: '261d6b1d-4e0d-45f1-96b2-eae3ea36e052', url: 'https://github.com/KevinOspina/node-hello.git']]])
            }
        }
        
        stage('Build'){
            steps{
                 
                    
                sh '''
                node --version
                npm --version
                npm install http-server
                npm install
                '''
                
                //sh 'docker login -u kevinospina03 -p ${PASS}'
                //sh 'cat ~/pass.txt | docker login --username kevinospina03 --password-stdin'
                //sh 'docker build .'
                //sh 'docker push kevinospina03/node_hello'
                
                 script {
                    /**
                     * login to docker for private repository
                     * create credentials in jenkins page.
                     **/
                     withCredentials([usernamePassword(credentialsId: '37267417-47b3-42ac-9844-3f307ddb9306', passwordVariable: 'password', usernameVariable: 'username')]){
                         sh '''
                            echo "${PASS} | docker login -u ${USER} --password-stdin"
                         '''
                         def app = docker.build("kevinospina03/hello_jest")
                        //app.push()
                     }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                
                sh '''
                npm run build
                npm run test'
                '''
                 withCredentials([usernamePassword(credentialsId: '37267417-47b3-42ac-9844-3f307ddb9306', passwordVariable: 'password', usernameVariable: 'username')]){
                    /**
                    * Restart docker server
                    **/
                    sh '''
                        echo "${PASS} | docker login -u ${USER} --password-stdin"
                        docker stop hello_jest
                        docker rm hello_jest
                        docker pull kevinospina03/hello_jest:latest
                        docker run -d -p 3000:3000 --name hello_jest -t kevinospina03/hello_jest:latest
                    '''
                }
            }
        
        }
    }
    
    
}
