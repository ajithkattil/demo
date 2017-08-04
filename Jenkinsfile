node {

  stage ('Checkout') {
    // checkout repository
    checkout scm

  }

  stage ('Docker Build') {
    // build Docker image 
    sh '/usr/local/bin/docker build . -t akattil-hellonode:${BUILD_ID}'
 
  
  
    //get existing Docker image info
     sh ' echo "List of all images" '
     sh ' /usr/local/bin/docker images '
    

    //If image building is a failure , send notification and exit
    sh ' echo "compiling code and building docker image is successful" '
    

    // get existing Docker container info
    sh 'echo " We have the following containers existing now" '
    sh '/usr/local/bin/docker ps'

  }

   stage ('Deploy') {
    // Containerisation of the image 
    
     //Remove the existing container if it exists
    sh '/usr/local/bin/docker  rm -f ajithcontainer && echo "container ajithcontainer removed" || echo "container ajithcontainer does not exist" '

     //Creating the new Docker Container 
    sh '/usr/local/bin/docker run -d --name ajithcontainer -p 8000:8000 akattil-hellonode:${BUILD_ID}'
    
     // Give the complete list of containers now
    sh 'echo " We have the following containers existing after the full run" '
    sh '/usr/local/bin/docker ps'
    
     // Verify the service at localhost:8000
    sh 'echo "Please verify the service availability at localhost:8000"'

  }

  
}

