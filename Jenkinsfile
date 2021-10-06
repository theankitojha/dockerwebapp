pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('dockerHub')
        SERVER_CREDENTIALS = credentials('serverKey')
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
                    remote.host = "192.168.0.201"
                    remote.allowAnyHosts = true
                    
                    echo "INFO: Deploy Stage"
                    withCredentials([
                        sshUserPrivateKey(credentialsId: 'serverKey', keyFileVariable: 'IDENTITY', usernameVariable: 'theankit', passwordVariable: '')
                    ]) {
                        remote.user = theankit
                        remote.identityFile = IDENTITY
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
