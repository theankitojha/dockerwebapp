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
                            sshUserPrivateKey(credentialsId: 'serverKey', usernameVariable: 'theankit', passwordVariable: '') 
                     
                        ]) {
                             remote.user = theankit
                            
                            
                        withCredentials ([
                            usernamePassword(credentialsId: 'dockerHub', usernameVariable: 'userName', passwordVariable: 'paswd')    
                        ]) {
                          
                          
                            sh ''' 
                              docker login https://index.docker.io/v1/ --username ${userName} --password ${paswd} '''
                         
                              
                              sh '''
                              docker rmi -f theankitojha/dockerwebapp
                              docker rm -f newcontainer
                              docker build -t theankitojha/dockerwebapp .
                              docker push theankitojha/dockerwebapp
                              '''
                            }
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
                        sshUserPrivateKey(credentialsId: 'serverKey', usernameVariable: 'theankit', passwordVariable: '')
                    ]) {
                        remote.user = theankit
                    
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
