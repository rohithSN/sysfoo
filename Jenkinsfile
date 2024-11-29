pipeline {
    agent any
    
    environment {
        DOCKER_REGISTRY = 'rohithsn/assignment'
        DOCKER_IMAGE = 'sysfoo'
        GIT_REPO = 'https://github.com/rohithSN/sysfoo.git'
        WAR_FILE_PATH = 'target/your-app.war'
        CREDENTIALS_ID = '1357b5d2-cb46-42b3-a150-463f6d960e7a' 
        GIT_CREDENTIALS_ID = '2c1047e2-11a3-4187-9b9b-eca511593caf'
    }
    
    stages {
       stage('Git Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/rohithSN/sysfoo.git'
            }
       }     
    }
}
