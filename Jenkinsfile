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
                          
                            sh '''
                              docker login https://index.docker.io/v1/ --username ${DOCKER_CREDENTIALS_USR} --password ${DOCKER_CREDENTIALS_PSW}
                              docker rmi -f theankitojha/dockerwebapp
                              docker rm -f newcontainer
                              docker build -t theankitojha/dockerwebapp .
                              docker push theankitojha/dockerwebapp
                              '''
                          
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
                  sshPublisher(publishers: [sshPublisherDesc(configName: 'sshHost', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''


               docker rm -f newcontainer
              docker run -d --name newcontainer theankitojha/dockerwebapp''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'abc.txt', usePty: true)], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                      
                  }  
                }
            }
        
        
    }
}
