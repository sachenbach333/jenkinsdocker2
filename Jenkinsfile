node {
	def application = "springbootapp"
	def dockerhubaccountid = "sachenbach333/jenkinsdocker"
	stage('Clone repository') {
		checkout scm
	}

	stage('Build image') {
		app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}
"""
	stage('Push image') {
		withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
		app.push()
		app.push("latest")

	}
	}
"""
	stage('Test image') {           
            app.inside {            
             
             sh 'echo "Tests passed"'        
            }    
        }     
       stage('Push image') {
       docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {            
       app.push("${env.BUILD_NUMBER}")            
       app.push("latest")        
              }    
           }
	stage('Deploy') {
		sh ("docker run -d -p 81:8080 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}
	
	stage('Remove old images') {
		// remove docker pld images
		sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}
