node {
    // reference to maven
    // ** NOTE: This 'maven-3.5.2' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'maven-3.5.2'

    // holds reference to docker image
    def dockerImage
    // ip address of the docker private repository(nexus)
 
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"
    
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/rhmanou/Jenkins-Test.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.5.2' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven-3.5.2'
    }    
  
    stage('Build Project') {
      // build project via maven
      //sh "'${mvnHome}/bin/mvn' clean install"
      sh "'${mvnHome}/bin/mvn' -B -DskipTests clean package"
    }
	
    /*stage('Initialize Docker'){         
	def dockerHome = tool 'MyDocker'         
	  env.PATH = "${dockerHome}/bin:${env.PATH}"     
    } */
    stage('Build Docker Image') {
	 agent {
	    docker {
		dockerfile true
		label 'docker'
	    }
	}
	steps{
		// build docker image
		dockerImage = docker.build("devopsexample:${env.BUILD_NUMBER}")
			//def dockerHome = tool 'MyDocker'         
			//env.PATH = "${dockerHome}/bin:${env.PATH}"  
			//sh "docker build -t devopsexample:${env.BUILD_NUMBER} ."
	}
    }
   
    stage('Deploy Docker Image'){
      
      // deploy docker image to nexus
		
      echo "Docker Image Tag Name: ${dockerImageTag}"
	  
	  sh "docker stop devopsexample"
	  
	  sh "docker rm devopsexample"
	  
	  sh "docker run --name devopsexample -d -p 2222:2222 devopsexample:${env.BUILD_NUMBER}"
	  
	  // docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
      //    dockerImage.push("${env.BUILD_NUMBER}")
      //      dockerImage.push("latest")
      //  }
      
    }
}
