
node {
    // reference to maven
    // ** NOTE: This 'maven-3.6.1' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'M2'
    // holds reference to docker image
    def dockerImage
    // ip address of the docker private repository(nexus)
    
    //def dockerRepoUrl = "https://registry.hub.docker.com"
    def dockerImageName = "hello-world-java"
    def dockerImageTag = "parosh/${dockerImageName}:${env.BUILD_NUMBER}"
    
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
     
      git 'https://github.com/paroshdey/docker-hello-world-spring-boot.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.6.1' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M2'
    }    
  
    stage('Build Project') {
      // build project via maven
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
    }
	
	
		
    stage('Build Docker Image') {
      // build docker image
     // sh "whoami"
      //sh "ls -all /var/run/docker.sock"
     // sh "mv ./target/hello*.jar ./data" 
      dockerImage = docker.build("parosh/hello-world-java")
    }
   
    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
        }
	    stage('deploy image'){
	    def app = openshift.newApp("parosh/hello-world-java:latest")
	    }
    }
}
