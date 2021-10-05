node {
    checkout scm

    docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {

        def customImage = docker.build("theankitojha/dockerwebapp")

        /* Push the container to the custom Registry */
        customImage.push()
    }
}
