pipeline {
  agent any

	parameters {
		gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH', type: 'PT_BRANCH'
	}

  stages {
    stage('Copy artifacts') {
      steps {
        copyArtifacts filter: 'multygo_master', fingerprintArtifacts: true, projectName: 'multygo/master', selector: lastSuccessful()
      }
    }
		stage('Deliver to prod') {
			when {
				expression {
					params.BRANCH == 'master'
				}
			}
            steps {
                ansiblePlaybook colorized: true,
                    
                    credentialsId: '4e9e04f8-0f41-4529-97c2-02f39c3bcb8a',
                    disableHostKeyChecking: true,
                    installation: 'asinble',
                    inventory: 'environments/production/host.ini',
                    playbook: 'playbook.yml'
            }
    }
        stage('tests prod') {
        when {
				expression {
					params.BRANCH == 'master'
				}
			}
            steps {
                sh 'docker run -v $HOME/workspace/Deploy/environments/staging:/etc/newman -t postman/newman run https://www.getpostman.com/collections/434a10daa020cc392009 -e prodoction.postman_environment.json'
                }
        }

        stage('staging') {
            when {
                expression {
                    params.BRANCH == 'staging'
            }
        }
            steps {
                ansiblePlaybook colorized: true,
                   
                    credentialsId: '4e9e04f8-0f41-4529-97c2-02f39c3bcb8a',
                    disableHostKeyChecking: true,
                    installation: 'asinble',
                    inventory: 'environments/staging/host.ini',
                    playbook: 'playbook.yml'
            }
    }

		stage('Test staging') {
			when {
				expression {
					params.BRANCH == 'staging'
				}
			}
            steps {
                sh 'docker run -v $HOME/workspace/Deploy/environments/staging:/etc/newman -t postman/newman run https://www.getpostman.com/collections/434a10daa020cc392009 -e staging.postman_environment.json'
			}
		}
	}
}
