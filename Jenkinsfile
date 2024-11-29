pipeline {
     agent {
        label 'slave'
    }
     tools {
        maven "Maven3"
    }
    
    environment {
        DOCKER_REGISTRY = 'rohithsn/assignment'
        DOCKER_IMAGE = 'sysfoo'
        DOCKER_TAG = ""
        REMOTE_HOST = '13.201.130.54' 
        REMOTE_USER = 'ubuntu'
    }
    
    stages {
       stage('Git Checkout - GitHub') {
            steps {
                git branch: 'master', url: 'https://github.com/rohithSN/sysfoo.git'
            }
        }
       stage('Build') {
            steps {
                script {
                    sh 'mvn clean install' 
                }
            }
        }
       stage('Determine Docker Tag') {
            steps {
                script {
                    DOCKER_TAG = "1.${env.BUILD_NUMBER}"
                    echo "Docker tag set to: ${DOCKER_TAG}"
                }
            }
        }  
       stage('Docker Build') {
            steps {
                sh "docker build -t rohithsn/assignment:${DOCKER_TAG} ."
            }
        }
       stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }  
      stage('Push Docker Image') {
            steps {
                sh "docker push rohithsn/assignment:${DOCKER_TAG}"
            }
        }

        stage('SSH into Remote Ubuntu Machine') {
            steps {
                sshagent(credentials: ['remote-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} << 'EOF'
                       
                        docker pull rohithsn/assignment:${DOCKER_TAG}
                        docker run -d --name my_cicd -p 9000:8080 rohithsn/assignment:${DOCKER_TAG}
                        EOF
                    """
                }
            }
        } 
    }
}
