  def remote = [:]
  remote.host = "192.168.0.202"
  remote.allowAnyHosts = true

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
                            sshUserPrivateKey(credentialsId: 'serverKey', keyFileVariable: 'IDENTITY', usernameVariable: 'theankit', passwordVariable: '') 
                            usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'userName', passwordVariable: 'paswd')
                        ]) {
                             remote.user = theankit
                             remote.identityFile = IDENTITY
                          
                            sh ''' 
                              docker login https://index.docker.io/v1/ --username ${userName} --password ${paswd}
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
