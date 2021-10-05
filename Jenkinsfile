node {
  checkout scm
  
  docker.withRegistry() {
    
    def customImage = docker.build("theankitojha/dockerwebapp")
    
    /* Push the container to the custom Registry */
    customImage.push()
  }
}
