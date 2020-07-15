pipeline {
    agent any
	
    stages {
      stage('Clone Sources') {
        steps {
          checkout scm
        } 
      }
  

	  stage('Prepare sources of the website') {
         steps {
	 sh '''
        cd docker
	    echo "Input param ${NAME}"		
	    echo "<p>Hello from ${NAME}!</p>" >> index.html
	 '''
         }
      }
	  
      stage('Build a docker image') {
         steps {
            echo 'Build process..'            
            sh '''
                cd docker
                docker build -t="mywebsite:${BUILD_NUMBER}" .
            '''
         }
      }
      stage('Deploy the website') {
         steps {
            echo 'Deploy process..'
			sh '''
				echo "Stopping running containers"
				CONTAINER=`docker ps -q`
				if [ -z "$CONTAINER" ]; then
					echo "No running containers. Nothing to stop"
				else									
					docker stop ${CONTAINER}
					docker rm ${CONTAINER}
				fi
				echo "Running a new container"
				docker run -d -p 80:80 ${REPO_NAME}:${BUILD_NUMBER}
				echo "Check the URL: http://`hostname`:80"
			'''
         }
      }
      
   }
}
