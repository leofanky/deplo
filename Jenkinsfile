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
            credentialsId: 'vagrant',
            disableHostKeyChecking: true,
            inventory: 'hosts.ini',
            playbook: 'playbook.yml'
        }
      }
    }
    stage('Run newman tests') {
      agent { docker { image "postman/newman"
                       args '--entrypoint=' } }
      steps {
        sh 'newman run https://www.getpostman.com/collections/a2d6a5db6868a3892c54 --reporters=cli,junit'
      }
    }
  }
}
