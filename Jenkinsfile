
pipeline{
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
  stages{
    stage('Build') {
      steps {
	sh 'rm -rf *.var'
        sh 'jar -cvf StudentSurvey.war -C "src/main/webapp" .'     
        sh 'docker build -t nikhila10/studentsurveyform:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW |docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
       }
    }
    stage("Push image to docker hub"){
      steps {
        sh 'docker push nikhila10/studentsurveyform:latest'
      }
    }
        stage("deploying on k8")
	{
		steps{
			sh 'kubectl set image deployment/assignment2-deployment container-0=nikhila10/studentsurveyform:latest -n default'
			sh 'kubectl rollout restart deploy assignment2-deployment -n default'
		}
	} 
  }
 
  post {
	  always {
			sh 'docker logout'
		}
	}    
}
