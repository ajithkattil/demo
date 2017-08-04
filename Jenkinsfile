node {

  stage ('Checkout') {
    // checkout repository
    checkout scm

  }

  stage ('Docker Build') {
    // build Docker image 
    sh '/usr/local/bin/docker build . -t akattil-hellonode:${BUILD_ID}'
  }
stage ('Docker image info') {
    //get Docker image info
  sh ' echo "List of all images" '
  sh ' /usr/local/bin/docker images '
    
  }
  
}
