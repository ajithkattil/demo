#!groovy
//Jenkinsfile is a groovy script DSL for defining CI/CD workflows for Jenkins
//node  allocates an executor and workspace for the Pipeline
//For best practices pls refer  https://www.cloudbees.com/blog/top-10-best-practices-jenkins-pipeline-plugin

  node {

  try {
         notifyBuild('STARTED')
  	       stage ('Checkout') {
            // checkout repository
            checkout scm
  	       }

  	        stage ('Docker Build') {
   	        // build Docker image 
   	        sh '/usr/local/bin/docker build . -t akattil-hellonode:${BUILD_ID}'
  	       }
  
 	   } catch (e) {
   	       // If there was an exception thrown, the build failed
    	       currentBuild.result = "FAILED"
   	       throw e
  	       } finally {
   	       // Success or failure, always send notifications
   	       notifyBuild(currentBuild.result)
  	   }
  
  	

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
  def notifyBuild(String buildStatus = 'STARTED') {
  	// build status of null means successful
  	buildStatus =  buildStatus ?: 'SUCCESSFUL'

  	// Default values
  	def colorName = 'RED'
  	def colorCode = '#FF0000'
  	def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  	def summary = "${subject} (${env.BUILD_URL})"

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
   
    //get existing Docker image info
  sh ' echo "List of all images" '
  sh ' /usr/local/bin/docker images '
    
  
    //If image building is a failure , send notification and exit
  sh ' echo "compiling code and building docker image is successful" '
    
  
    // get existing Docker container info
    sh 'echo " We have the following containers existing now" '
    sh '/usr/local/bin/docker ps'

  

   stage ('Deploy') {
    // Containerisation of the image 
    
     //Remove the existing container if it exists
    sh '/usr/local/bin/docker  rm -f ajithcontainer && echo "container ajithcontainer removed" || echo "container 			ajithcontainer does not exist" '

     //Creating the new Docker Container 
    	sh '/usr/local/bin/docker run -d --name ajithcontainer -p 8000:8000 akattil-hellonode:${BUILD_ID}'
    
    	 // Give the complete list of containers now
    	sh 'echo " We have the following containers existing after the full run" '
    	sh '/usr/local/bin/docker ps'
    
     	// Verify the service at localhost:8000
   	 sh 'echo "Please verify the service availability at localhost:8000"'

  	}

  
}
