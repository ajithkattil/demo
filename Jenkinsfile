node {

  stage ('Checkout') {
    // checkout repository
    sh ' echo "Checking out the project from  from Github repository"'
    checkout scm

  }

  stage ('Docker Build') {
    // build Docker image 
    sh ' echo "build , package , and ultimately creating Docker image"'
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
    sh ' echo "Deploying the image , ie running to create a Docker container of the image"'
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

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"
  def details = """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
    <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>"""

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)

  emailext(
      subject: subject,
      body: details,
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}
