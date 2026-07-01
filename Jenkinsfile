pipeline {

    agent any

    environment {
        IMAGE = "dockerhubusername/petclinic"
    }

    stages {

     

        stage('Build') {
            steps {
              sh './mvnw clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE:latest .'
            }
        }

        stage('Docker Push') {

            steps {

                withCredentials([usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                )]) {

                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE:latest
                    '''
                }
            }

        }

    }

}