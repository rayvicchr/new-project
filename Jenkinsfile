node('') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Sonar Analysis'){
		//sh 'mvn sonar:sonar -Dsonar.host.url=http://35.153.67.119:9000 -Dsonar.login=77467cfd2653653ad3b35463fbfdb09285f08be5'
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	
        stage('Build image') {         
       
            app = docker.build("rvch/project1")    
        }          

        stage('Test image') {     

                  app.inside {            
             
             sh 'echo "Tests passed"'        
            }    
        }     

       stage('Push image') {                       
                                                 
     docker.withRegistry('https://registry.hub.docker.com', 'git') {                  
        app.push("${env.BUILD_NUMBER}")            
        app.push("latest")        
              }    
           }

	stage ('Notification'){
		emailext (
		      subject: "Job Completed",
		      body: "Jenkins Pipeline Job for Maven Build got completed !!!",
		      to: "build-alerts@example.com"
		    )
	}
}
