pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('dockerHub')
    }
    stages {

        stage("Build")
        {
            steps
            {
                script {
                        echo "INFO: Build Stage"
                        withCredentials ([
                            usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'USER', passwordVariable: 'PSWD')    
                        ]) {
                            sh '''
                            docker login https://index.docker.io/v1/ --username ${USER} --password ${PSWD}
                            docker build -t theankitojha/dockerwebapp .
                            docker push theankitojha/dockerwebapp
                            '''
                           }
                        }
            }
        }

        
    }
}
