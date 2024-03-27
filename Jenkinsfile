node () {

     stage("checkout from SCM") {
         
         git 'https://github.com/santhoshvarma800/MavenProfilesProject.git'
         
        }
     
     stage("Maven Build ") {
	 
	   def mvnHome = tool name: 'M2_HOME', type: 'maven'
	   def mvnCmd = "${mvnHome}/bin/mvn"
	   sh "${mvnCmd} clean deploy"
    }
  
     stage (" Build Docker image ") {
      
       sh "docker build -t santhoshvarma800/tomcatintegration:1.0 ."	  
         
     }
     
     stage ( " Pushing Docker Image ") {
	
	      withCredentials([string(credentialsId: 'pwd', variable: 'Dockerhubpwd')]) {
    
	            sh "docker login -u santhoshvarma800 -p ${Dockerhubpwd}"
            }
      sh "docker push santhoshvarma800/tomcatintegration:1.0"
     }

    
    stage ("Remove Existing Container") {
	  
	  def stopContainer = "docker stop tomcatcontainer"
	  def removeContainer = "docker rm tomcatcontainer"
	  
	  try {
	  
	  sshagent(['dev-pem']) {
		
		sh "ssh -o StrictHostKeyChecking=no ec2-user@13.201.53.49 ${stopContainer}"
		sh "ssh -o StrictHostKeyChecking=no ec2-user@13.201.53.49 ${removeContainer}"
		
         }
	  }catch(error) {
	     
		 sh "No container exists"
	  
        }
   }
    
    
       stage ( " Create Container ") {
	
	       def runContainer = "docker run -itd --name tomcatcontainer -p 8080:8080 santhoshvarma800/tomcatintegration:1.0"
	    
		   sshagent(['dev-pem']) {
		
		    sh "ssh -o StrictHostKeyChecking=no ec2-user@13.201.53.49 ${runContainer}"
   
         }
	}
	



}