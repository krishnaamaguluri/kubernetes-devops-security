pipeline 
{
  agent any

  stages 
  {
      stage('Build Artifact') 
      {
            steps 
            {
              	sh "mvn clean package -DskipTests=true"
              	archive 'target/*.jar' 
        	}  
      }

      stage('Unit Tests') 
      {
      	steps 
      	{
        	sh "mvn test"
      	} 
      }

    stage('Unit Tests - JUnit and Jacoco') 
    {
      steps 
      {
        sh "mvn test"
      }
      post 
      {
       	 always 
       	 {
          	junit 'target/surefire-reports/*.xml'
          	jacoco execPattern: 'target/jacoco.exec'
        	}
     	}
	}

	stage('Docker Build and push')
	{
		steps
		{
			withDockerRegistry([credentialsId: "docker-hub", url: ""]) 
			{
				sh "printenv"
				sh 'docker build -t krishna4646/numeric-app:""$GIT_COMMIT"" .'
          		sh 'docker push krishna4646/numeric-app:""$GIT_COMMIT""'
			}
		}

	}

	stage('Kunerbetes Deployment - Dev')
	{
		steps
		{
			withKubeConfig([credentialsId: 'kubeconfig']){
				sh "sed -i 's#replace#krishna4646/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
				sh "kubectl apply -f k8s_deployment_service.yaml "
			}
		}
	}

	 
}
}
