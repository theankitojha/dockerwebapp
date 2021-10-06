pipeline {
    agent any
    environment {
        SERVER_CREDENTIALS = credentials('server-cred')
        DOCKER_CREDENTIALS = credentials('docker-cred')
    }
    stages {

        stage("Build")
        {
            steps
            {
                script {
                        echo "INFO: Build Stage"
                        withCredentials ([
                            usernamePassword(credentials: 'docker-cred', usernameVariable: USER, passwordVariable: PWD)    
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

        stage("Deploy")
        {
            steps
            {
                script {
                            echo "INFO: Deploy Stage"
                    withCredentials ([
                        usernamePassword(credentials: 'server-cred', usernameVariable: USER, passwordVariable: PWD)
                    ]) {
                        sh '''
                        docker rm -f newcontainer
                        docker run -d --name newcontainer theankitojha/dockerwebapp
                        '''
                    }
                            
                       }
            }
        }
    }
}
