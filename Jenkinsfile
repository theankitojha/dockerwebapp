pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('dockerHub')
        SERVER_CREDENTIALS = credentials('server-cred')
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
                            docker rmi -f theankitojha/dockerwebapp
                            docker rm -f newcontainer
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
                script 
                {
                    
                    def remote = [:]
                    remote.host = "192.168.0.202"
                    remote.allowAnyHosts = true
                    
                    echo "INFO: Deploy Stage"
                    withCredentials([
                        usernamePassword(credentialsId: 'server-cred', usernameVariable: 'USER', passwordVariable: 'PSWD')
                    ]) {
                        remote.user = USER
                        remote.password = PSWD
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
