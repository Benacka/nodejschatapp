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
        snykSecurity(
            snykInstallation: 'Snyk',
            snykTokenId: 'SNYKID',
            severity: 'critical'
          )
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
            def app = docker.build("benjast/nodejschatapp")
            app.tag("latest")
         }
      }
    }
 
      stage('POST-TO-DOCKERHUB')
    {
        steps
      {
         script
         {
            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials2')
            {
              def app = docker.image("benjast/nodejschatapp")
              app.push('latest')
            }
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
 
 
