pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub-creds')
    }

    stages {
        stage('Clone') {
            steps {
                sh '''
                    if [ ! -d "register-app-full-devops" ]; then
                        git clone https://github.com/Tanaji199/register-app-full-devops.git
                    else
                        echo "Directory already exists. Skipping clone."
                    fi
                '''
            }
        }

        stage('Maven Build') {
            steps {
                dir('register-app-full-devops') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Dockerize Image') {
            steps {
                dir('register-app-full-devops') {
                    sh 'docker build -t myapp .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                    echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                    docker tag myapp:latest tanaji199/myapp:latest
                    docker push tanaji199/myapp:latest
                '''
            }
        }
    }
}
