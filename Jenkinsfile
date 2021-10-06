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
                            usernamePassword(credentials: 'dockerHub', usernameVariable: USER, passwordVariable: PWD)    
                        ]) {
                            sh '''
                            docker login --username ${USER} --password ${PWD}
                            docker build -t theankitojha/dockerwebapp .
                            docker push theankitojha/dockerwebapp
                            '''
                           }
                        }
            }
        }

        
    }
}
