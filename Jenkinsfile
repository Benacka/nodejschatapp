pipeline
{
  agent none
 
  stages
  {
    stage('CLONE GIT REPOSITORY')
    {
      agent
      {
        label 'ubuntu-Appserver-2'
      }
      steps
      {
        checkout scm
      }
    }
 
    stage('SCA-SAST-SNYK-TEST')
    {
      agent
      {
        label 'ubuntu-Appserver-2'
      }
      steps
      {
        echo "SNYK-TEST"
      }
    }
 
     stage('BUILD-AND-TAG')
    {
      agent
      {
        label 'ubuntu-Appserver-2'
      }
      steps
      {
         script
         {
            def app = docker.build("benacka/nodejschatapp")
            app.tag("latest")
         }
      }
    }
 
      stage('POST-TO-DOCKERHUB')
    {
        steps
      {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials2')
            {
                app.push("latest")
            }
      }
    }
 
    stage('DEPLOYMENT')
    {
      agent
      {
        label 'ubuntu-Appserver-2'
      }
      steps
      {
        sh "docker-compose down"
        sh "docker-compose up -d"
      }
    }
 
   
   
  }
 
}
 
 
