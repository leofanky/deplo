pipeline {
  agent any
  stages {
    stage('Copy artifacts') {
      steps {
        copyArtifacts filter: 'multygo_master', fingerprintArtifacts: true, projectName: 'multygo/master', selector: lastSuccessful()
      }
    }
    stage('Run ansible') {
      steps {
          ansiblePlaybook colorized: true,
            credentialsId: '4e9e04f8-0f41-4529-97c2-02f39c3bcb8a',
            disableHostKeyChecking: true,
            installation: 'asinble',
            inventory: '/var/lib/jenkins/workspace/example1-deployment/host.ini',
            playbook: '/var/lib/jenkins/workspace/example1-deployment/playbook.yml'
        
      }
    }
    stage('Run newman tests') {
      agent { 
        docker { 
          image "postman/newman"
          args '--entrypoint=' 
        }
      }
      steps {
        sh 'newman run https://www.getpostman.com/collections/434a10daa020cc392009 --reporters=cli,junit'
      }
    }
  }
}
