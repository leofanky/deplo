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
        ansiColor('xterm') {
          ansiblePlaybook colorized: true,
            credentialsId: '4e9e04f8-0f41-4529-97c2-02f39c3bcb8a',
            disableHostKeyChecking: true,
            installation: 'asinble',
            inventory: '/var/lib/jenkins/workspace/example1-deployment/host.ini',
            playbook: '/var/lib/jenkins/workspace/example1-deployment/playbook.yml'
        }
      }
    }
  
  }
}
