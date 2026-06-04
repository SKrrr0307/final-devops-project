pipeline {

    agent any

    stages {

        stage('Build Docker Image') {

            steps {
                echo "Building image versioning ${BUILD_NUMBER}"
                sh 'docker build -t skrrr0307/final-devops-project:${BUILD_NUMBER} .'
            }

        }

        stage('Login Docker Hub') {

            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-hub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {

                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                '''
               }

            }

        }

        stage('Push Image') {

            steps {
                sh 'docker push skrrr0307/final-devops-project:${BUILD_NUMBER}'
            }

        }
        
        stage('Deploy Container') {
            
            steps {
                sh '''
                docker stop final-devops || true
                docker rm final-devops || true 


                docker run -d --name final-devops -p 8081:80 skrrr0307/final-devops-project:${BUILD_NUMBER}
                ''' 
            } 

        }
 

    }

}
