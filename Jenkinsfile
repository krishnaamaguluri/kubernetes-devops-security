pipeline 
{
  agent any

  stages 
  {
      

	 stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t krishna4646/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push krishna4646/numeric-app:""$GIT_COMMIT""'
        }
      }
    }
}
}
