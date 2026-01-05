pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "tirupatipallu/java1"
        DOCKER_TAG   = "latest"
    }

    tools {
        //jdk 'jdk-17'
        maven 'maven-3.9.6'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Tirupatitata123/new-java.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh '''
                    mvn clean package -DskipTests
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                '''
            }
        }

        
       stage('Login to Docker Hub') {
    steps {
      sh "docker login -u tirupatipallu -p Mech.dec@1212" 
    }
}


        stage('Push Image to Docker Hub') {
            steps {
                sh '''
                    docker push $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    
    stage("Deploy to Container") {
            steps {
                script {
                    // Stop & remove old container (if exists)
                    sh """
                    if [ \$(docker ps -aq -f name=java1) ]; then
                        docker rm -f java1
                    fi
                    """

                    // Run latest container
                    sh """
                    docker run -d --name java1 -p 3000:3000 tirupatipallu/java1
                    """
                }
            }
    }
    }

    post {
        success {
            echo 'Docker Image Build & Push Successful 🚀'
        }
        failure {
            echo 'Pipeline Failed ❌'
        }
        always {
            sh 'docker logout'
        }
    }
}
