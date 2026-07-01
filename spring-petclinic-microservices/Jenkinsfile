pipeline {
    agent any
    environment {
        IMAGE = "dockerhubusername/petclinic"
    }
    stages {
      stage('Build') {
            steps {
                dir('spring-petclinic-microservices') {
                    sh 'chmod +x mvnw'
                    sh './mvnw clean package -DskipTests'
                }
            }
        }
        stage('Docker Build') {
            steps {
                dir('spring-petclinic-microservices') {
                   sh 'docker build -f docker/Dockerfile -t $IMAGE:latest .'
                }
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
