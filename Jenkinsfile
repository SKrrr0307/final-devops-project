pipeline {

    agent any

    stages {

        stage('Build Docker Image') {

            steps {
                sh 'docker build -t skrrr0307/final-devops-project:v1 .'
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
                sh 'docker push skrrr0307/final-devops-project:v1'
            }

        }

    }

}
