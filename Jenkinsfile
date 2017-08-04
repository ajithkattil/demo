node {

  stage ('Checkout') {
    // checkout repository
    checkout scm

  }

  stage ('Docker Build') {
    // build Docker image 
    sh '/usr/local/bin/docker build . -t akattil-hellonode:${BUILD_ID}'
  }

}
