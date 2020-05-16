pipeline {
  agent any

	stages {
		stage('Copy artifacts') {
			steps {
				copyArtifacts filter: 'mypipeisdestryedatthispoint', fingerprintArtifacts: true, projectName: 'mypipeisdestryedatthispoint', selector: lastSuccessful()
      				}
    		}
	  stage('Deliver to prod') {
			steps {
                ansiblePlaybook colorized: true,    
                    credentialsId: 'aws',
                    disableHostKeyChecking: true,
                    installation: 'asinble',
                    inventory: 'environments/production/host.ini',
                    playbook: 'playbook.yml'
        	}
    	}
	}
}
