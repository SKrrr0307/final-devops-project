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
                string(
                credentialsId: 'docker-hub-creds',
                variable: 'DOCKER_PASS'
                )
                ]) {

                sh '''
                echo $DOCKER_PASS | docker login \
                -u skrrr0307 \
                --password-stdin
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
